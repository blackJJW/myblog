---
title: "[Java] 콘솔 입출력, System.out 필드"
description: ""
date: "2023-01-06T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 콘솔에서 입력된 데이터를 `System.in`으로 읽었다면, 반대로 콘솔로 데이터를 출력하기 위해서는 System 클래스의 out 정적 필드를 사용
    - out은 PrintStream 타입의 필드
    - PrintStream이 OutputStream의 하위 클래스
        - out 필드를 OutputStream 타입으로 변환해서 사용 가능
    
    ```java
    OutputStream os = System.out;
    ```
    
- 콘솔로 1개의 바이트를 출력하려면 OutputStream의 `write(int b)` 메소드를 이용하면 된다.
    - 이때 바이트 값은 아스키 코드
    - `write()` 메소드는 아스키 코드를 문자로 콘솔에 출력
    
    ex) 아스키 코드 97번을 `write(int b)` 메소드로 출력하면 ‘a’가 출력
    
    ```java
    byte b = 97;
    os.write(b);
    os.flush();
    ```
    
- OutputStream의 `write(int b)` 메소드는 1바이트만 보낼 수 있기 때문에 1바이트로 표현 가능한 숫자, 영어, 특수문자는 출력이 가능
    - 2바이트로 표현되는 한글은 출력 불가
- 한글을 출력하기 위해서는 우선 한글을 바이트 배열로 얻은 다음
    - `write(byte[] b)`나 `write(byte[] b, int off, int len)` 메소드로 콘솔에 출력하면 된다.
    
    ```java
    String name = "가나다";
    byte[] nameBytes = name.getBytes();
    os.write(nameBytes);
    os.flush();
    ```
    
- ex) `write(int b)` 메소드를 사용해서 연속된 숫자, 영어를 출력
    - `write(byte[] b)` 메소드를 사용해서 한글을 출력
    - SystemOutEx.java
    
    ```java
    import java.io.OutputStream;
    
    public class SystemOutEx {
    
    	public static void main(String[] args) throws Exception {
    		OutputStream os = System.out;
    		
    		for(byte b = 48; b < 58; b++) {
    			// 아스키 코드 48에서 57까지의 문자를 출력
    			os.write(b);
    		}
    		// 라인피드(10)을 출력하면 다음 행으로 넘어간다.
    		os.write(10);
    		
    		for(byte b = 97; b < 123; b++) {
    			// 아스키 코드 97에서 122까지의 문자를 출력
    			os.write(b);
    		}
    		os.write(10);
    		
    		String hangul = "가나다라마바사아자차카타파하";
    		byte[] hangulBytes = hangul.getBytes();
    		os.write(hangulBytes);
    		
    		os.flush();
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/System_out_필드/Untitled.png)
    
- System 클래스의 out 필드를 OutputStream 타입으로 콘솔에 출력하는 작업은 그리 편하지 않다.
    - out은 원래 PrintStream 타입의 필드
        - PrintStream의 `print()` 또는 `println()` 메소드를 사용하면 좀 더 쉬운 방법으로 다양한 타입의 데이터를 콘솔에 출력할 수 있다.
    
    ```java
    PrintStream ps = System.out;
    ps.println(...);
    ```
    
    ```java
    System.out.println(...);
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판