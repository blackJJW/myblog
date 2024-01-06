---
title: "[Java] String 클래스, String 생성자"
description: ""
date: "2022-09-24T18:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 문자열은 데이터로서 많이 사용된다.
- 문자열을 생성하는 방법과 추출, 비교, 찾기, 분리, 변환 등을 제공하는 메소드를 익혀야한다.

# String 생성자

---

- 자바의 문자열은 `java.lang` 패키지의 String 클래스의 인스턴스로 관리
- 소스상에서 문자열 리터럴은 String 객체로 자동 생성
    - String 클래스의 다양한 생성자를 이용해서 직접 String 객체를 생성 가능
- String 클래스는 Deprecated(비권장)된 생성자를 제외하고 약 13개의 생성자를 제공
    - Deprecated는 예전 자바 버전에서는 사용되었으나, 그 후 버전에서는 사용하지 말라는 의미
- 어떤 생성자를 이용해서 STring 객체를 생성할 지는 제공되는 매개값의 타입에 달려 있다.
    
    ### 사용 빈도수가 높은 생성자
    
    - 파일의 내용을 읽거나, 네트워크를 통해 받은 데이터는 보통 `byte[]` 배열이므로 이것을 문자열로 변환되기 위해 사용된다.
    
    ```java
    // 배열 전체를 String 객체로 생성
    String str = new String(byte[] bytes);
    
    // 지정한 문제셋으로 디코딩
    String str = new String(byte[] bytes, String charsetName);
    
    // 배열의 offset 인덱스 위치부터 length만큼 String 객체로 생성'
    String str = new String(byte[] bytes, int offset, int length);
    
    // 지정한 문자셋으로 디코딩
    String str = new String(byte[] bytes, int offset, int length, String charsetName)
    ```
    
- ex) 바이트 배열을 문자열로 변환
    - ByteToStringEx.java
    
    ```java
    public class ByteToStringEx {
    
    	public static void main(String[] args) {
    		byte[] bytes = {72, 101, 108, 108, 111, 32, 74, 97, 118, 97};
    		
    		String str1 = new String(bytes);
    		System.out.println(str1);
    		
    		String str2 = new String(bytes, 6, 4);
    		                          // 74 위치, 4개 
    		System.out.println(str2);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/String_클래스,_String_생성자/Untitled.png)
    
- ex) 키보드로 부터 읽은 바이트 배열을 문자열로 변환하는 방법
    - `System.in.read()` 메소드는 키보드에서 입력한 내용을 매개값으로 주어진 바이트 배열에 저장하고 읽은 바이트 수를 리턴
        - ex) Hello를 입력하고 `Enter` 키를 눌렀을 경우
            - Hello + 캐리지리턴(`\r`) + 라인피드(`\n`)의 코드 값이 바이트 배열에 저장
            - 총 7개의 바이트를 읽었기 때문에 7을 리턴
            
            ![Untitled](/images/lang_java/basicAPI/String_클래스,_String_생성자/Untitled%201.png)
            
    - 영어는 알파벳 한 자가 1바이트로 표현
        - 한글과 기타 다른 나라 언어는 2바이트로 표현
        - 입력된 문자 수와 읽은 바이트 수가 다를 수 있다.
- ex) 바이트 배열을 문자열로 변환
    - 바이트 배열을 문자열로 변환하기 위해 `String(byte[] bytes, int offset, int length)`를 사용
    
    ```java
    import java.io.IOException;
    
    public class KeyboardToStringEx {
    
    	public static void main(String[] args) throws IOException{
    		// 읽은 바이트를 저장하기 위한 배열 생성
    		byte[] bytes = new byte[10];
    		
    		System.out.println("input : ");
    		
    		//배열에 읽은 바이트를 저장하고 읽은 바이트수를 리턴
    		int readByteNo = System.in.read(bytes);
    		
    		// 배열을 문자열로 변환
    		String str = new String(bytes, 0, readByteNo - 2);
                                       // 2를 뺀 이유 : \r + \n 부분은
    		                           // 문자열로 만들 필요가 없기 때문
    		System.out.println(str);
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/String_클래스,_String_생성자/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판