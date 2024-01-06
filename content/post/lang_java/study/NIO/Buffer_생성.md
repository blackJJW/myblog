---
title: "[Java] NIO, Buffer 생성"
description: ""
date: "2023-01-24T11:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"

---
<!--more-->

- 각 데이터 타입별로 넌다이렉트 버퍼를 생성하기 위해서는 각 Buffer 클래스의 `allocate()`와 `wrap()` 메소드를 호출

## 1. `allocate()` 메소드

- JVM 힙 메모리에 넌다이렉트 버퍼를 생성
    
    ### 데이터 타입별로 Buffer를 생성하는 `allocate()` 메소드
    
    | 리턴 타입 | 메소드(매개 변수) | 설명 |
    | --- | --- | --- |
    | ByteBuffer | ByteBuffer.allocate(int capacity) | capacity개만큼의 byte 값을 저장 |
    | CharBuffer | CharBuffer.allocate(int capacity) | capacity개만큼의 char 값을 저장 |
    | DoubleBuffer | DoubleBuffer.allocate(int capacity) | capacity개만큼의 double 값을 저장 |
    | FloatBuffer | FloatBuffer.allocate(int capacity) | capacity개만큼의 float 값을 저장 |
    | IntBuffer | IntBuffer.allocate(int capacity) | capacity개만큼의 int 값을 저장 |
    | LongBuffer | LongBuffer.allocate(int capacity) | capacity개만큼의 long 값을 저장 |
    | ShortBuffer | ShortBuffer.allocate(int capacity) | capacity개만큼의 short 값을 저장 |
- 최대 100개의 바이트를 저장하는 ByteBuffer를 생성
    - 최대 100개의 문자를 저장하는 CharBuffer를 생성하는 코드
    
    ```java
    ByteBuffer byteBuffer = ByteBuffer.allocate(100);
    CharBuffer charBuffer = CharBuffer.allocate(100);
    ```
    

## 2. `wrap()` 메소드

- 각 타입별 Buffer 클래스는 모두 `wrap()` 메소드를 가지고 있다.
    - `wrap()` 메소드는 이미 생성되어 있는 자바 배열을 래핑헤서 Buffer 객체를 생성
    - 자바 배열은 JVM 힙 메모리에 생성되므로 `wrap()`은 넌다이렉트 버퍼를 생성
- 길이가 100인 `byte[]`를 이용해서 ByteBuffer를 생성
    - 길이가 100인 `char[]`를 이용해서 CharBuffer를 생성
    
    ```java
    byte[] byteArr = new byte[100];
    ByteBuffer byteBuffer = ByteBuffer.wrap(byteArray);
    
    char[] charArray = new char[100];
    CharBuffer charBuffer = CharBuffer.wrap(charArray);
    ```
    
- 배열의 모든 데이터가 아니라 일부 데이터만 가지고 Buffer 객체를 생성할 수도 있다.
    - 이 경우 시작 인덱스와 길이를 추가적으로 지정
    
    ex) 다음은 0 인덱스부터 50개만 버퍼로 생성
    
    ```java
    byte[] byteArray = new byte[100];
    ByteBuffer byteBuffer = ByteBuffer.wrap(byteArray, 0, 50);
    
    char[] charArray = new char[100];
    CharBuffer charBuffer = CharBuffer.wrap(charArray, 0, 50);
    ```
    
- CharBuffer는 추가적으로 CharSequence 타입의 매개값을 갖는 `wrap()` 메소드도 제공
    - String이 CharSequence 인터페이스를 구현했기 때문에 매개값으로 문자열을 제공해서 CharBuffer를 생성할 수도 있다.
    
    ```java
    CharBuffer charBuffer = CharBuffer.wrap("ABC");
    ```
    

## 3. `allocateDirect()` 메소드

- ByteBuffer의 `allocateDirect()` 메소드는 JVM 힙 메모리 바깥쪽, 즉 운영체제가 관리하는 메모리에 다이렉트 버퍼를 생성
- 이 메소드는 각 타입별 Buffer 클래스에는 없고, ByteBuffer에서만 제공
- 각 타입별로 다이렉트 다이렉트 버퍼를 생성하고 싶다면 우선 ByteBuffer의 `allocateDirect()` 메소드로 버퍼를 생성
    - ByteBuffer의 `asCharBuffer()`, `asShortBuffer()`, `asIntBuffer()`, `asLongBuffer()`, `asFloatBuffer()`, `asDoubleBuffer()` 메소드를 이용해서 해당 타입별 Buffer를 얻는다.
- ex)
    - 다음을 생성하는 코드
        - 100개의 바이트(byte)를 저장하는 다이렉트 ByteBuffer
        - 50개의 문자(char)를 저장하는 다이렉트 CharBuffer
        - 25개의 정수(int)를 저장하는 다이렉트 IntBuffer
    - char는 2바이트 크기를 가지고, int는 4바이트 크기를 가지기 때문에 초기 다이렉트 ByteBuffer 생성 크기에 따라 저장 용량이 결정
    
    ```java
    // 100개의 byte값 저장
    ByteBuffer byteBuffer = ByteBuffer.allocateDirect(100);
    
    // 50개의 char값 저장
    CharBuffer charBuffer 
    		= ByteBuffer.allocateDirect(100).asCharBuffer();
    
    // 25개의 int값 저장
    IntBuffer intBuffer
    		= ByteBuffer.allocateDirect(100).asIntBuffer();
    ```
    
    - DirectBufferCapacityEx.java
    
    ```java
    import java.nio.ByteBuffer;
    import java.nio.CharBuffer;
    import java.nio.IntBuffer;
    
    public class DirectBufferCapacityEx {
    
    	public static void main(String[] args) {
    		// 100개의 byte값 저장
    		ByteBuffer byteBuffer = ByteBuffer.allocateDirect(100);
    		System.out.println("저장용랼 : " + byteBuffer.capacity() + " 바이트");
    		
    		// 50개의 char값 저장
    		CharBuffer charBuffer 
    				= ByteBuffer.allocateDirect(100).asCharBuffer();
    		System.out.println("저장용랼 : " + charBuffer.capacity() + " 문자");
    		
    		// 25개의 int값 저장
    		IntBuffer intBuffer
    				= ByteBuffer.allocateDirect(100).asIntBuffer();
    		System.out.println("저장용랼 : " + intBuffer.capacity() + " 정수");
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/Buffer_생성/Untitled.png)
    

## 4. byte 해석 순서(ByteOrder)

- 데이터를 처리할 때 바이트 처리 순서는 운영체제마다 차이가 있다.
    - 이러한 차이는 데이터를 다른 운영체제로 보내거나 받을 때 영향을 미치기 때문에 데이터를 다루는 버퍼도 이를 고려해야 한다.
- 앞쪽 바이트부터 먼저 처리하는 것을 Big endian
- 뒤쪽 바이트부터 먼저 처리하는 것을 Littel endian
    
    ![Untitled](/images/lang_java/NIO/Buffer_생성/Untitled%201.png)
    
    - Little endian으로 동작하는 운영체제에서 만든 데이터 파일을 Big endian으로 동작하는 운영체제에서 읽는다면 ByteOrder 클래스로 데이터 순서를 맞춰야 한다.
- ByteOrder 클래스의 `nativeOrder()` 메소드는 현재 동작하고 있는 운영체제가 Big endian인지 Little endian인지 알려준다.
    - JVM도 일종의 독립된 운영체제
        - 이런 문제를 취급
        - JRE가 설치된 어떤 환경이든 JVM은 무조건 Big endian으로 동작
- ex) 현재 컴퓨터의 운영체제 종류와 바이트를 해석하는 순서에 대해 출력
    - ComputerByteOrderEx.java
    
    ```java
    import java.nio.ByteOrder;
    
    public class ComputerByteOrderEx {
    
    	public static void main(String[] args) {
    		System.out.println("운영체제 종류 : " 
    												+ System.getProperty("os.name"));
    		System.out.println("네이티브의 바이트 해석 순서 : " 
    												+ ByteOrder.nativeOrder());
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/Buffer_생성/Untitled%202.png)
    
    - 운영체제와 JVM의 바이트 해석 순서가 다를 경우에는 JVM이 운영체제와 데이터를 교환할 때 자동적으로 처리해주기 때문에 문제가 없다.
    - 다이렉트 버퍼일 경우 운영체제의 native I/O를 사용하므로 운영체제의 기본 해석 순서로 JVM의 해석 순서를 맞추는 것이 성능에 도움이 된다.
        - `allocateDirect()`로 버퍼를 생성한 후, `order()` 메소드를 호출해서 `nativeOrder()`의 리턴값으로 세팅
    
    ```java
    ByteBuffer byteBuffer 
    	= ByteBuffer.allocateDirect(100).order(ByteOrder.nativeOrder());
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판