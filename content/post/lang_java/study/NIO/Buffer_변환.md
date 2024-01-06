---
title: "[Java] NIO, Buffer 변환"
description: ""
date: "2023-01-26T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"

---
<!--more-->

- 채널이 데이터를 읽고 쓰는 버퍼는 모두 ByteBuffer
    - 채널을 통해 읽은 데이터를 복원하려면 ByteBuffer를 문자열 또는 다른 타입 버퍼로 변환해야 한다.
        - 다른 타입 버퍼
            - CharBuffer, ShortBuffer, IntBuffer, LongBuffer, FloatBuffer, DoubleBuffer
    - 반대로 문자열 또는 다른 타입 버퍼의 내용을 채널을 통해 쓰고 싶다면 ByteBuffer로 변환해야 한다.

## 1. ByteBuffer ↔ String

- 프로그램에서 가장 많이 처리되는 데이터는 String 타입, 즉 문자열
- 채널을 통해 문자열을 파일이나 네트워크로 전송하려면 특정 문자셋(UTF-8, EUC-KR)으로 인코딩해서 ByteBuffer로 변환해야 한다.
- 먼저 문자셋을 표현하는 `java.nio.charset.Charset` 객체가 필요
    
    ```java
    // 매개값으로 주어진 문자셋
    Charset charset = Charset.forName("UTF-8");
    
    // 운영체제가 사용하는 디폴트 문자셋
    Charset charset = Charset.defaultCharset(); 
    ```
    
- Charset을 이용해서 문자열을 ByteBuffer로 변환하려면 `encode()` 메소드를 호출
    
    ```java
    String data = ...;
    ByteBuffer byteBuffer = charset.encode(data);
    ```
    
    - 반대로 파일이나 네트워크로부터 읽은 ByteBuffer가 특정 문자셋(UTF-8, EUC-KR)으로 인코딩되어 있을 경우
        - 해당 문자셋으로 디코딩해야만 문자열로 복원할 수 있다.
- Charset은 ByteBuffer를 디코딩해서 CharBuffer로 변환시키는 `decode()` 메소드를 제공하고 있기 때문에 문자열로 쉽게 복원할 수 있다.
    
    ```java
    ByteBuffer byteBuffer = ...;
    String data = charset.decode(byteBuffer).toString();
    ```
    
- ex) 문자열을 UTF-8로 인코딩해서 얻은 ByteBuffer를 다시 UTF-8로 디코딩해서 문자열로 복원
    - ByteBufferToStringEx.java
    
    ```java
    import java.nio.ByteBuffer;
    import java.nio.charset.Charset;
    
    public class ByteBufferToStringEx {
    
    	public static void main(String[] args) {
    		Charset charset = Charset.forName("UTF-8");
    		
    		// 문자열 -> 인코딩 -> ByteBuffer
    		String data = "안녕하세요";
    		ByteBuffer byteBuffer = charset.encode(data);
    		
    		// ByteBuffer -> 디코딩 -> CharBuffer -> 문자열
    		data = charset.decode(byteBuffer).toString();
    		System.out.println("문자열 복원 : " + data);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/Buffer_변환/Untitled.png)
    

## 2. ByteBuffer ↔ IntBuffer

- `int[]` 배열을 생성하고 이것을 파일이나 네트워크로 출력하기 위해서는 `int[]` 배열 또는 IntBuffer로부터 ByteBuffer를 생성해야 한다.
- int 타입은 4byte 크기를 가지므로 `int[]` 배열 크기 또는 IntBuffer의capacity보다 4배 큰 capacity를 가진 ByteBuffer를 생성
    - ByteBuffer의 `putInt()` 메소드로 정수값을 하나씩 저장
- ex) int[] 배열을 IntBuffer로 래핑(꼭 IntBuffer로 래핑할 필요는 없다.)
    - 4배 큰 ByteBuffer를 생성한 후 정수값을 저장
    - `putInt()` 메소드는 position을 이동시키기 때문에 모두 저장할 후에는 position을 0으로 되돌려 놓는 `flip()` 메소드를 호출해야 한다.
    
    ```java
    int[] data = new int[] { 10, 20 };
    IntBuffer intBuffer = IntBuffer.wrap(data);
    ByteBuffer byteBuffer 
    			= ByteBuffer.allocate(intBuffer.capacity() * 4);
    for(int i = 0; i < intBuffer.capacity(); i++) {
    	byteBuffer.putInt(intBuffer.get(i));
    }
    byteBuffer.flip();
    ```
    
- ex) 파일이나 네트워크로부터 입력된 ByteBuffer에 4바이트씩 연속된 Int 데이터가 저장되어 있을 경우
    - `int[]` 배열로 복원이 가능
    - ByteBuffer의 `asIntBuffer()` 메소드로 IntBuffer를 얻는다.
        - IntBuffer의 capacity와 동일한 크기의 `int[]` 배열을 생성
        - IntBuffer의 `get()` 메소드로 int값들을 배열에 저장
    
    ```java
    ByteBuffer byteBuffer = ...;
    IntBuffer intBuffer = byteBuffer.asIntBuffer();
    int[] data = new int[intBuffer.capacity()];
    intBuffer.get(data);
    ```
    
    - ByteBuffer에서 `asIntBuffer()`로 얻은 IntBuffer에서는 `array()` 메소드를 사용해서 `int[]` 배열을 얻을 수 없다.
        - `array()` 메소드는 래핑한 배열만 리턴
        - `int[]` 배열을 `wrap()` 메소드로 래핑한 IntBuffer에서만 사용할 수 있다.
- ex) `int[]` 배열로부터 얻은 ByteBuffer를 이용해서 다시 `int[]` 배열로 복원
    - ByteBufferToIntBufferEx.java
    
    ```java
    import java.nio.ByteBuffer;
    import java.nio.IntBuffer;
    import java.util.Arrays;
    
    public class ByteBufferToIntBufferEx {
    
    	public static void main(String[] args) throws Exception {
    		// int[] -> IntBuffer -> ByteBuffer
    		int[] writeData = { 10, 20 };
    		IntBuffer writeIntBuffer = IntBuffer.wrap(writeData);
    		ByteBuffer writeByteBuffer 
    			= ByteBuffer.allocate(writeIntBuffer.capacity() * 4);
    		for(int i = 0; i < writeIntBuffer.capacity(); i++) {
    			writeByteBuffer.putInt(writeIntBuffer.get(i));
    		}
    		writeByteBuffer.flip();
    		
    		// ByteBuffer -> IntBuffer -> int[]
    		ByteBuffer readByteBuffer = writeByteBuffer;
    		IntBuffer readIntBuffer = readByteBuffer.asIntBuffer();
    		int[] readData = new int[readIntBuffer.capacity()];
    		readIntBuffer.get(readData);
    		System.out.println("배열 복원 : " + Arrays.toString(readData));
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/Buffer_변환/Untitled%201.png)
    
- ByteBuffer와 IntBuffer 간의 변환을 이해하면 ByteBuffer와 ShortBuffer, LongBuffer, FloatBuffer, DoubleBuffer 간의 변환도 비슷한 방법으로 수행
    
    ### ByteBuffer가 가지고 있는 기본 타입 버퍼로 변환하는 asXXXBuffer() 메소드
    
    | 리턴 타입 | 변환 메소드 | 설명 |
    | --- | --- | --- |
    | ShortBuffer | asShortBuffer() | 2바이트씩 연속된 short 데이터를 가진 ByteBuffer일 경우 |
    | IntBuffer | asIntBuffer() | 4바이트씩 연속된 int 데이터를 가진 ByteBuffer일 경우 |
    | LongBuffer | asLongBuffer() | 8바이트씩 연속된 long 데이터를 가진 ByteBuffer일 경우 |
    | FloatBuffer | asFloatBuffer() | 4바이트씩 연속된 float 데이터를 가진 ByteBuffer일 경우 |
    | DoubleBuffer | asDoubleBuffer() | 8바이트씩 연속된 double 데이터를 가진 ByteBuffer일 경우 |

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판