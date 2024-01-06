---
title: "[Java] TCP 네트워킹, Socket 데이터 통신"
description: ""
date: "2023-01-20T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "Network"
---
<!--more-->

- 클라이언트가 연결 요청(`connect()`)하고 서버가 연결 수락(`accept()`)했을 경우
    - 양쪽의 Socket 객체로부터 각각 입력 스트림(InputStream)과 출력 스트림(OutputStream)을 얻을 수 있다.
    
    ![Untitled](/images/lang_java/inputOutput/Socket_데이터_통신/Untitled.png)
    
- Socket으로부터 InputStream과 OutputStream을 얻는 코드
    
    ```java
    // 입력 스트림 얻기
    InputStream is = socket.getInputStream();
    
    // 출력 스트림 얻기
    OutputStream os = socket.getOutputStream();
    ```
    
- 상대방에게 데이터를 보내기 위해서는 보낼 데이터를 `byte[]` 배열로 생성
    - 이것을 매개값으로 해서 OutputStream의 `write()` 메소드를 호출하면 된다.
- 문자열을 UTF-8로 인코딩한 바이트 배열을 얻어내고, `write()` 메소드로 전송
    
    ```java
    String data = "보낼 데이터";
    byte[] byteArr = data.getBytes("UTF-8");
    OutputStream outputStream = socket.getOutputStream();
    outputStream.write(byteArr);
    outputStream.flush();
    ```
    
- 상대방이 보낸 데이터를 받기 위해서는 받은 데이터를 저장할 `byte[]` 배열을 하나 생성
    - 이것을 매개값으로 해서 InputStream의 `read()` 메소드를 호출
    - `read()` 메소드는 읽은 데이터를 byte[] 배열에 저장하고 읽은 바이트 수를 리턴
    - 다음은 데이터를 읽고 UTF-8로 디코딩한 문자열을 얻는 코드
    
    ```java
    byte[] byteArr = new byte[100];
    InputStream inputStream = socket.getInputStream();
    int readByteCount = inputStream.read(byteArr);
    String data = new String(byteArr, 0, readByteCount, "UTF-8");
    ```
    
- ex) 연결 성공 후
    1. 클라이언트가 먼저 “Hello Server”를 서버로 보낸다.
    2. 서버가 이 데이터를 받는다.
    3. “Hello Client”를 클라이언트로 보낸다.
    4. 클라이언트가 이 데이터를 받는다.
    - ClientEx.java
    
    ```java
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.OutputStream;
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
    			
    			byte[] bytes = null;
    			String message = null;
    			
    			// 1. -------------------------------------------
    			OutputStream os = socket.getOutputStream();
    			message = "Hello Server";
    			bytes = message.getBytes("UTF-8");
    			os.write(bytes);
    			os.flush();
    			System.out.println(" [ 데이터 보내기 성공 ] ");
    			//-----------------------------------------------
    			
    			// 4. -------------------------------------------
    			InputStream is = socket.getInputStream();
    			bytes = new byte[100];
    			int readByteCount = is.read(bytes);
    			message = new String(bytes, 0, readByteCount, "UTF-8");
    			System.out.println(" [ 데이터 받기 성공 ] " + message);
    			
    			os.close();
    			is.close();
    			
    		} catch(Exception e) {}
    		
    		if(!socket.isClosed()) { // 연결되어 있을 경우
    			try {
    				socket.close(); // 연결 끊기
    			} catch(IOException e1) {}
    		}
    	}
    }
    ```
    
    - ServerEx.java
    
    ```java
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.OutputStream;
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
    				
    				byte[] bytes = null;
    				String message = null;
    				
    				// 2.-------------------------------------------------------
    				InputStream is = socket.getInputStream();
    				bytes = new byte[100];
    				int readByteCount = is.read(bytes);
    				message = new String(bytes, 0, readByteCount, "UTF-8");
    				System.out.println(" [데이터 받기 성공]" + message);
    				//--------------------------------------------------
    				
    				// 3.-----------------------------------------------
    				OutputStream os = socket.getOutputStream();
    				message = "Hello Client";
    				bytes = message.getBytes("UTF-8");
    				os.write(bytes);
    				os.flush();
    				System.out.println(" [ 데이터 보내기 성공 ] ");
    				
    				is.close();
    				os.close();
    				socket.close();
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
    
    ![Untitled](/images/lang_java/inputOutput/Socket_데이터_통신/Untitled%201.png)
    
    - ServerEx.java부터 실행하고 ClientEx.java를 실행
- 데이터를 받기 위해 InputStream의 `read()` 메소드를 호출하면 상대발이 데이터를 보내기 전까지는 블로킹(blocking)된다.
    - `read()` 메소드가 블로킹 해제되고 리턴되는 경우는 다음 세 가지
    
    | 블로킹이 해제되는 경우 | 리턴값 |
    | --- | --- |
    | 상대방이 데이터를 보냄 | 읽은 바이트 수 |
    | 상대방이 정상적으로 Socket의 close()를 호출 | -1 |
    | 상대방이 비정상적으로 종료 | IOException 발생 |
- 상대방이 정상적으로 Socket의 `close()`를 호출하고 연결을 끊었을 경우와 비정상적으로 종료했을 경우
    - 모두 예외 처리를 해서 이쪽도 Socket을 닫기 위해 `close()` 메소드를 호출해야 한다.
    
    ```java
    try {
    	...
    	// 상대방이 비정상적으로 종료했을 경우 IOException 발생
    	int readByteCount = inputStream.read(byteArr);
    
    	// 상대방이 정상적으로 Socket의 close()를 호출했을 경우
    	if(readByteCount == -1) {
    		throw new IOException(); // 강제로 IOException 발생시킴
    	}
    	...
    } catch (Exception e) {
    	try { socket.close(); } catch(Exception e2) {}
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판