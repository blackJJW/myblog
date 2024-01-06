---
title: "[Java] TCP 네트워킹, ServerSocket 생성과 연결 수락"
description: ""
date: "2023-01-18T23:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "Network"
---
<!--more-->

- 서버를 개발하려면 우선 ServerSocket 객체를 얻어야 한다.
- ServerSocket을 얻는 가장 간단한 방법은 생성자에 바인딩 포트를 대입하고 객체를 생성하는 것
    - ex) 5001번 포트에 바인딩하는 ServerSocket을 생성
    
    ```java
    ServerSocket serverSocket = new ServerSocket(5001);
    ```
    
- ServerSocket을 얻는 다른 방법은 디폴트 생성자로 객체를 생성하고 포트 바인딩을 위해 `bind()` 메소드를 호출하는 것
    - `bind()` 메소드의 매개값은 포트 정보를 가진 `InetSocketAddress()`
    
    ```java
    ServerSocket serverSocket = new ServerSocket();
    serverSocket.bind(new InetSocketAddress(5001));
    ```
    
- 만약 서버 PC에 멀티 IP가 할당되어 있을 경우
    - 특정 IP로 접속할 때만 연결을 수락하고 싶다면 다음과 같이 작성
    - “localhost” 대신 정확한 IP를 주면 된다.
    
    ```java
    ServerSocket serverSocket = new ServerSocket();
    serverSocket.bind( new InetSocketAddress("localhost", 5001) );
    ```
    
- ServerSocket을 생성할 때 해당 포트가 이미 다른 프로그램에서 사용 중이라면 `BindException`이 발생
    - 이 경우에는 다른 포트로 바인딩하거나, 다른 프로그램을 종료하고 다시 실행하면 된다.
- 포트 바인딩까지 끝났다면 ServerSocket은 클라이언트 연결 수락을 위해 `accept()` 메소드를 실행해야 한다.
    - `accept()` 메소드는 클라이언트가 연결 요청하기 전까지 블로킹된다.
        - 블로킹 : 스레드가 대기 상태가 된다.
    - UI를 생성하는 스레드나, 이벤트를 처리하는 스레드에서 `accept()` 메소드를 호출하지 않도록 한다.
        - 블로킹이 되면 UI 갱신이나 이벤트 처리를 할 수 없기 때문
    - 클라이언트가 연결 요청을 하면 `accept()` 는 클라이언트와 통신할 Socket을 만들고 리턴
        - 이것이 연결 수락
- 만약 accept()에서 블로킹되어 있을 때 ServerSocket을 닫기 위해 `close()` 메소드를 호출하면 SocketException이 발생
    - 예외 처리가 필요
    
    ```java
    try {
    	Socket socket = serverSocket.accept();
    } catch(Exception e) {}
    ```
    
- 연결된 클라이언트의 IP와 포트 정보를 알고 싶을 경우
    - Socket의 `getRemoteSocketAddress()` 메소드를 호출해서 SocketAddress를 얻는다.
    - 실제 리턴되는 것은 InetSocketAddress 객체
        - 다음과 같이 타입 반환할 수 있다.
    
    ```java
    InetSocketAddress socketAddress 
        = (InetSocketAddress) socket.getRemoteSocketAddress();
    ```
    
    ### InetSocketAddress에는 IP와 포트 정보를 리턴하는 메소드
    
    | 리턴 타입 | 메소드명(매개 변수) | 설명 |
    | --- | --- | --- |
    | String | getHostName() | 클라이언트 IP 리턴 |
    | int | getPort() | 클라이언트 포트 번호 리턴 |
    | String | toString() | “IP 포트번호” 형태의 문자열 리턴 |
- 더 이상 클라이언트 연결 수락이 필요 없으면 ServerSocket의 `close()` 메소드를 호출해서 포트를 언바인딩시켜야 한다.
    - 다른 프로글매에서 해당 포트를 재사용할 수 있게 된다.
    
    ```java
    serverSocket.close();
    ```
    
- ex) 반복적으로 accept() 메소드를 호출해서 다중 클라이언트 연결을 수락하는 가장 기본적인 코드
    - ServerEx.java
    
    ```java
    import java.io.IOException;
    import java.net.InetSocketAddress;
    import java.net.ServerSocket;
    import java.net.Socket;
    
    public class ServerEx {
    
    	public static void main(String[] args) {
    		ServerSocket serverSocket = null;
    		try {
    			// ServerSocket 생성
    			serverSocket = new ServerSocket();
    			serverSocket.bind(new InetSocketAddress("localhost", 5001));
    			
    			while(true) {
    				System.out.println("[ 연결 기다림 ]");
    				
    				// 클라이언트 연결 수락
    				Socket socket = serverSocket.accept();
    				
    				InetSocketAddress isa = (InetSocketAddress) socket.getRemoteSocketAddress();
    				System.out.println("[ 연결 수락함 ]" + isa.getHostName());
    			}
    		} catch(Exception e) {}
    		
    		// ServerSocket이 닫혀있지 않을 경우
    		if(!serverSocket.isClosed()) {
    			try {
    				// ServerSocket 닫기
    				serverSocket.close();
    			} catch (IOException e1) {}
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/ServerSocket_생성과_연결_수락/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판