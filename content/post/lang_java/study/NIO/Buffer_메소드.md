---
title: "[Java] NIO, Buffer 메소드"
description: ""
date: "2023-01-26T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"

---
<!--more-->

- Buffer를 생성한 후 사용할 때에는 Buffer가 제공하는 메소드를 잘 활용해야 한다.
    - 공통적으로 사용하는 메소드
    - 데이터 타입별로 Buffer가 개별적으로 가지고 있는 메소드

## 1. 공통 메소드

- 각 타입별 버퍼 클래스는 Buffer 추상 클래스를 상속
- Buffer 추상 클래스에는 모든 버퍼가 공통적으로 가져야 할 메소드들이 정의
    - 위치 속성을 변경하는 `flip()`, `rewind()`, clear(), `mark()`, `reset()`
    
    ### Buffer 추상 클래스가 가지고 있는 메소드
    
    | 리턴 타입 | 메소드(매개 변수) | 설명 |
    | --- | --- | --- |
    | Object | array() | 버퍼가 래핑(wrap)한 배열을 리턴 |
    | int | arrayOffset() | 버퍼의 첫 번째 요소가 있는 내부 배열의 인덱스를 리턴 |
    | int | capacity() | 버퍼의 전체 크기를 리턴 |
    | Buffer | clear() | 버퍼의 위치 속성을 초기화(position=0, limit=capacity) |
    | Buffer | flip() | limit을 position으로, position을 0 인덱스로 이동 |
    | boolean | hasArray() | 버퍼가 래핑(warp)한 배열을 가지고 있는지 여부 |
    | boolean | hasRemaining() | position과 limit사이에 요소가 있는지 여부(position < limit) |
    | boolean | isDirect() | 운영체제의 버퍼를 사용하는지 여부 |
    | boolean | isReadOnly() | 버퍼가 읽기 전용인지 여부 |
    | int | limit() | limit 위치를 리턴 |
    | Buffer | limit(int newLimit) | newLimit으로 limit 위치를 설정 |
    | Buffer | mark() | 현재 위치를 mark로 표시 |
    | int | position() | position 위치를 리턴 |
    | Buffer | position(int newPosition) | newPosition으로 position 위치를 설정 |
    | int | remaining() | position과 limit 사이의 요소의 개수 |
    | Buffer | reset() | position을 mark 위치로 이동 |
    | Buffer | rewind() | position을 0 인덱스로 이동 |

## 2. 데이터를 읽고 저장하는 메소드

- 버퍼에 데이터를 저장하는 메소드는 `put()`
    - 데이터를 읽는 메소드는 `get()`
    - 이 메소드들은 Buffer 추상 클래스에는 없다.
        - 각 타입별 하위 Buffer 클래스가 가지고 있다.
- `get()`과 `put()` 메소드는 상대적(Relative)과 절대적(Absolute)으로 구분
    - 버퍼 내의 현재 위치 속성인 position에서 데이터를 읽고, 저장할 경우는 상대적
    - position과 상관없이 주어진 인덱스에서 데이터를 읽고, 저장할 경우는 절대적
    - 상대적 `get()`과 `put()` 메소드를 호출하면 position의 값은 증가
    - 절대적 `get()`과 `put()` 메소드를 호출하면 position의 값은 증가되지 않는다.
    - 만약 position 값이 limit 값까지 증가했는데도 상대적 `get()`을 사용하면 BufferUnderflowException 예외가 발생
        - `put()` 메소드를 사용하면 BufferOverflowException 예외가 발생
    
    ### ByteBuffer와 CharBuffer에서 제공하는 `get()` 메소드와 `put()` 메소드의 표
    
    ![Untitled](/images/lang_java/NIO/Buffer_메소드/Untitled.png)
    
- 상대적 메소드와 절대적 메소드를 쉽게 구분하는 방법
    - index 매개 변수가 없으면 상대적
    - index 매개 변수가 있으면 절대적

## 3. 버퍼 예외의 종류

- 버퍼 클래스에서 발생하는 예외
    - 주로 버퍼가 다 찼을 때 데이터를 저장하려는 경우
    - 버퍼에서 더 이상 읽어올 데이터가 없을 때 데이터를 읽으려는 경우
    
    ### 버퍼와 관련된 예외 클래스
    
    - 이 중에서 가장 흔하게 발생하는 예외는 BufferOverflowException과 BufferUnderflowException이다.
    
    | 예외 | 설명 |
    | --- | --- |
    | BufferOverflowException | position이 limit에 도달했을 때 put()을 호출하면 발생 |
    | BufferUnderflowException | position이 limit에 도달했을 때 get()을 호출하면 발생 |
    | InvalidMarkException | mark가 없는 상태에서 reset() 메소드를 호출하면 발생 |
    | ReadOnlyBufferException | 읽기 전용 버퍼에서 put() 또는 compact() 메소드를 호출하면 발생 |
- ex) 데이터를 버퍼에 쓰고, 읽을 때, 그리고 위치 속성을 변경하는 메소드를 호출할 때 버퍼의 위치 속성값의 변화를 보여줌
    - BufferEx.java
    
    ```java
    import java.nio.Buffer;
    import java.nio.ByteBuffer;
    
    public class BufferEx {
    
    	public static void main(String[] args) {
    		System.out.println("[ 7바이트 크기로 버퍼 생성 ]");
    		
    		// 다이렉트 버퍼 생성
    		ByteBuffer buffer = ByteBuffer.allocate(7);
    		printState(buffer);
    		
    		// 상대적 저장
    		buffer.put((byte) 10);
    		buffer.put((byte) 11);
    		System.out.println("[ 2바이트 저장후 ]");
    		printState(buffer);
    		
    		// 상대적 저장
    		buffer.put((byte) 12);
    		buffer.put((byte) 13);
    		buffer.put((byte) 14);
    		System.out.println("[ 3바이트 저장후 ]");
    		printState(buffer);
    		
    		// 데이터를 읽기 위해 위치 속성값 변경
    		buffer.flip();
    		System.out.println("[ flip() 실행 후 ]");
    		printState(buffer);
    		
    		// 상대적 읽기
    		buffer.get(new byte[3]);
    		System.out.println("[ 3바이트 읽은 후 ]");
    		printState(buffer);
    		
    		// 마크하기
    		buffer.mark();
    		System.out.println("-----[ 현재 위치를 마크 해놓음 ]");
    		
    		// 상대적 읽기
    		buffer.get(new byte[2]);
    		System.out.println("[ 2바이트 읽은 후 ]");
    		printState(buffer);
    		
    		// 마크 위치로 position 이동
    		buffer.reset();
    		System.out.println("-----[ position을 마크 위치로 옮김 ]");
    		printState(buffer);
    		
    		// position을 0으로 이동
    		buffer.rewind();
    		System.out.println("[ rewind() 실행후 ]");
    		printState(buffer);
    		
    		// 모든 위치 속성값을 초기화
    		buffer.clear();
    		System.out.println("[ clear() 실행 후 ]");
    		printState(buffer);
    
    	}
    	
    	public static void printState(Buffer buffer) {
    		System.out.print("\tposition : " + buffer.position() + ", ");
    		System.out.print("\tlimt : " + buffer.limit() + ", ");
    		System.out.println("\tcapacity : " + buffer.capacity());
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/Buffer_메소드/Untitled%201.png)
    
- ex) `compact()` 메소드 호출 후, 변경된 버퍼의 내용과 position, limit의 위치를 보여줌
    - CompactEx.java
    
    ```java
    import java.nio.ByteBuffer;
    
    public class CompactEx {
    
    	public static void main(String[] args) {
    		System.out.println("[ 7바이트 크기로 버퍼 생성 ]");
    		ByteBuffer buffer = ByteBuffer.allocateDirect(7);
    		buffer.put((byte)10);
    		buffer.put((byte)11);
    		buffer.put((byte)12);
    		buffer.put((byte)13);
    		buffer.put((byte)14);
    		
    		// 데이터를 읽기 위해 위치 속성값 변경
    		buffer.flip();
    		printState(buffer);
    		
    		buffer.get(new byte[3]);
    		System.out.println("[ 3바이트 읽음 ]");
    		
    		// 읽지 않은 데이터는 0 인덱스부터 복사
    		buffer.compact();
    		System.out.println("[ compact() 실행 후 ]");
    		printState(buffer);
    	}
    	
    	public static void printState(ByteBuffer buffer) {
    		System.out.print(buffer.get(0) + ", ");
    		System.out.print(buffer.get(1) + ", ");
    		System.out.print(buffer.get(2) + ", ");
    		System.out.print(buffer.get(3) + ", ");
    		System.out.println(buffer.get(4));
    		System.out.print("position : " + buffer.position() + ", ");
    		System.out.print("limit : " + buffer.limit() + ", ");
    		System.out.println("capacity : " + buffer.capacity());
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/Buffer_메소드/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판