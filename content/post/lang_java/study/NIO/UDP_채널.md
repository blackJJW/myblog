---
title: "[Java] NIO, UDP 채널"
description: ""
date: "2023-02-05T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "Network"

---
<!--more-->

## 1. 발신자 만들기

- DatagramChannel을 생성하려면 `open()` 메소드를 호출해야 한다.
- `open()` 메소드는 ProtocolFamily 인터페이스 타입의 매개값을 가진다.
    - 이 객체의 역할은 IPv4와 IPv6를 구분하기 위함
    - 구현 객체는 StandardProtocolFamily 열거 상수를 사용하면 된다.
    - IPv4를 사용하는 DatagramChannel을 생성하는 코드
    
    ```java
    DatagramChannel datagramChannel
    	= DatagramChannel.open(StandardProtocolFamily.INET);
    ```
    
- DatagramChannel을 이용해서 데이터를 보내기 위해서는 `send()` 메소드를 이용
    - 첫 번째 매개값은 보낸 데이터를 가지고 있는 ByteBuffer
    - 두 번째 매개값은 수신자 IP와 포트 정보를 가지고 있는 SocketAddress
        - SocketAddress는 추상 클래스이므로 하위 클래스인 InetSocketAddress 객체를 생성하고 대입
    - `send()` 메소드의 리턴값은 실제로 보낸 바이트 수
    - 로컬 PC 5001번을 수신자로하고 데이터를 전송
    
    ```java
    int byteCount = datagramChannel.send(byteBuffer,
    	new InetSocketAddress("localhost", 5001));
    ```
    
- 더 이상 보낼 데이터가 없을 경우에는 DatagramChannel을 닫기 위해 `close()` 메소드를 호출
    
    ```java
    datagramChannel.close();
    ```
    
- ex) UDP 발신 프로그램
    - for문은 두 번 반복하는데 각각 “메시지1”, “메시지2” 문자열을 전송
    - UdpSendEx.java
    
    ```java
    import java.net.InetSocketAddress;
    import java.net.StandardProtocolFamily;
    import java.nio.ByteBuffer;
    import java.nio.channels.DatagramChannel;
    import java.nio.charset.Charset;
    
    public class UdpSendEx {
    
    	public static void main(String[] args) throws Exception {
    		DatagramChannel datagramChannel
    			= DatagramChannel.open(StandardProtocolFamily.INET);
    		
    		System.out.println("[ 발신 시작 ]");
    		
    		for(int i = 1; i < 3; i++) {
    			String data = "메시지" + i;
    			Charset charset = Charset.forName("UTF-8");
    			ByteBuffer byteBuffer = charset.encode(data);
    			
    			// 데이터 보내기
    			int byteCount = datagramChannel.send(
    					byteBuffer,
    					new InetSocketAddress("localhost", 5001)
    			);
    			System.out.println("[ 보낸 바이트 수 ] " + byteCount + " bytes");
    		}
    		System.out.println("[ 발신 종료 ]");
    		
    		datagramChannel.close();
    	}
    }
    ```
    

## 2. 수신자 만들기

- DatagramChannel을 이용해서 데이터를 받기 위해서는 `bind()` 메소드를 호출해서 포트와 먼저 바인딩 필요
    - 매개값은 SocketAddress 타입으로 InetSocketAddress 객체를 대입
    
    ```java
    DatagramChannel datagramChannel 
    	= DatagramChannel.open(StandardProtocolFamily.INET);
    datagramChannel.bind(new InetSocketAddress(5001));
    ```
    
- 포트 바인딩이 되엇다면 `receive()` 메소드로 데이터를 받을 수 있다.
    - `receive()` 메소드의 매개값은 받은 데이터를 저장할 ByteBuffer
    - 리턴 타입은 원격 클라이언트의 IP와 포트 정보를 가지고 있는 SocketAddress
        - 실제로는 InetSocketAddress가 리턴
    
    ```java
    SocketAddress socketAddress 
    	= datagramChannel.receive(ByteBuffer dst);
    ```
    
    - 데이터를 받기 전까지 `receive()` 메소드는 블로킹
        - 데이터를 받으면 리턴
- 수신자는 항상 데이터를 받을 준비를 해야 하므로 작업 스레드를 생성해서 `receive()` 메소드를 반복적으로 호출해야 한다.
- 작업 스레드를 종료시키는 방법은 두 가지
    - receive() 메소드가 블로킹되어 있는 상태에서 작업 스레드의 `interrupt()`를 호출시켜 ClosedByInterruptException 예외를 발생
    - DatagramChannel의 `close()`를 호출시켜 AsynchronousCloseException 예외를 발생시키는 것
        - 예외 처리 코드에서 작업 스레드를 종료시킨다.
    
    ```java
    datagramChannel.close();
    ```
    
- ex) UdpReceiveEx.java
    
    ```java
    import java.net.InetSocketAddress;
    import java.net.SocketAddress;
    import java.net.StandardProtocolFamily;
    import java.nio.ByteBuffer;
    import java.nio.channels.DatagramChannel;
    import java.nio.charset.Charset;
    
    public class UdpReceiveEx extends Thread{
    
    	public static void main(String[] args) throws Exception{
    		DatagramChannel datagramChannel
    			= DatagramChannel.open(StandardProtocolFamily.INET);
    		datagramChannel.bind(new InetSocketAddress(5001));
    		
    		// 스레드 생성
    		Thread thread = new Thread() {
    			@Override
    			public void run() {
    				System.out.println("[ 수신 시작 ]");
    				try {
    					while(true) {
    						ByteBuffer byteBuffer = ByteBuffer.allocate(100);
    						// 데이터 받기
    						SocketAddress socketAddress = datagramChannel.receive(byteBuffer);
    						byteBuffer.flip();
    						
    						Charset charset = Charset.forName("UTF-8");
    						String data = charset.decode(byteBuffer).toString();
    						System.out.println("[ 받은 내용 : " + socketAddress.toString() + " ] " + data);
    					}
    				} catch (Exception e) {
    					System.out.println("[ 수신 종료 ]");
    				}
    			}
    		};
    		thread.start();
    		
    		Thread.sleep(10000);
    		datagramChannel.close();
    	}
    }
    ```
    

## 3. 수신자와 발신자 실행

- 수신자를 먼저 실행해야 한다.
- 발신자를 먼저 실행하면 수신자가 실행하기 전에 보낸 데이터는 받을 수 없다.
    
    ![Untitled](/images/lang_java/NIO/UDP_채널/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판