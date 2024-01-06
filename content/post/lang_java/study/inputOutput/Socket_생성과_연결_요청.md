---
title: "[Java] TCP 네트워킹, Socket 생성과 연결 요청"
description: ""
date: "2023-01-19T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "Network"
---
<!--more-->

- 클라이언트가 서버에 연결을 요청을 하려면 `java.net.Socket`을 이용해야 한다.
- Socket 객체를 생성함과 동시에 연결 요청을 하려면 생성자의 매개값으로 서버의 IP 주소와 바인딩 포트 번호를 제공하면 된다.
- 로컬 PC의 5001 포트에 연결 요청하는 코드
    
    ```java
    try {
       Socket socket = new Socket("localhost", 5001); // 방법 1
       Socket socket 
             = new Socket( new InetSocketAddress("localhost", 5001 ); // 방법 2
    } catch (UnknownHostException e) {
       // IP 표기 방법이 잘못되었을 경우
    } catch (IOException e) {
       // 해당 포트의 서버에 연결할 수 없는 경우
    }
    ```
    
    - 외부 서버에 접속하려면 localhost 대신 정확한 IP를 입력
    - IP 대신 도메인 이름만 알고 있을 경우
        - 도메인 이름을 IP 주소로 번역해야 한다.
            - InetSocketAddress 객체를 이용하는 방법을 사용
- Socket 생성과 동시에 연결 요청을 하지 않고, 기본 생성자로 Socket을 생성한 후,  `connect()` 메소드로 연결 요청
    
    ```java
    socket = new Socket();
    socket.connect( new InetSocketAddress("localhost", 5001 );
    ```
    
- 연결 요청을 할 때는 두 가지 예외가 발생할 수 있다.
    1. `UnknownHostException`은 잘못 표기된 IP 주소를 입력했을 때 발생
    2. `IOException`은 주어진 포트로 접속할 수 없을 때 발생
    - 두 가지 예외 처리가 필요
- Socket 생성자 및 `connect()` 메소드는 서버와 연결이 될 때까지 블로킹된다.
    - UI를 생성하는 스레드나, 이벤트를 처리하는 스레드에서 Socket 생성자 및 `connect()`를 호출하지 않도록 한다.
        - 블로킹이 되면 UI 갱신이나 이벤트 처리를 할 수 없기 때문
    - 연결된 후, 클라이언트 프로그램을 종료하거나, 강제적으로 연결을 끊고 싶을 경우 Socket의 `close()` 메소드를 호출
        - `close()` 메소드로 IOException이 발생할 수 있기 때문에 예외 처리가 필요
    
    ```java
    try {
       socket.close();
    } catch ( IOException e ) { } 
    ```
    
- ex) localhost 5001 포트로 연결을 요청하는 코드
    - `connect()` 메소드가 정상적으로 리턴하면 연결이 성공한 것
    - ClientEx.java
    
    ```java
    import java.io.IOException;
    import java.net.InetSocketAddress;
    import java.net.Socket;
    
    public class ClientEx {
    
    	public static void main(String[] args) {
    		Socket socket = null;
    		try {
    			socket = new Socket(); // Socket 생성
    			System.out.println(" [ 연결 요청 ] ");
    			
    			// 연결 요청
    			socket.connect(new InetSocketAddress("localhost", 5001));
    			System.out.println(" [ 연결 성공 ] ");
    		} catch(Exception e) {}
    		
    		if(!socket.isClosed()) { // 연결되어 있을 경우
    			try {
    				socket.close(); // 연결 끊기
    			} catch(IOException e1) {}
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/Socket_생성과_연결_요청/Untitled.png)
    
- ex) 연결을 수락하는 코드
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
    
    - 먼저 이 코드를 실행하고 위 ClientEx.java를 실행한다.
    
    ![Untitled](/images/lang_java/inputOutput/Socket_생성과_연결_요청/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판