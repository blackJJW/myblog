---
title: "[Java] String 클래스, String 메소드, 바이트 배열로 변환(getBytes())"
description: ""
date: "2022-09-25T20:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 종종 문자열을 바이트 배열로 변환하는 경우가 존재
    - ex) 네트워크로 문자열을 전송, 문자열을 암호화할 때 문자열을 바이트 배열로 변환
- 문자열을 바이트 배열로 변환하는 메소드
    
    ```java
    byte[] bytes = "문자열".getBytes();
    byte[] bytes = "문자열".getBytes(Charset charset);
    ```
    
    - getBytes() 메소드는 시스템의 기본 문자셋으로 인코딩된 바이트 배열을 리턴
    - 특정 문자셋으로 인코딩된 바이트 배열을 얻으려면 두 번째 메소드를 사용하면 된다.
- ex) EUC-KR과 UTF-8로 각각 인코딩된 바이트 배열을 리턴
    
    ```java
    try{
    	byte[] bytes = "문자열".getBytes("EUC-KR");
    	byte[] bytes = "문자열".getBytes("UTF-8");
    } catch (UnsupportedEncodingException e){
    }
    ```
    
    - 어떤 문자셋으로 인코딩하느냐에 따라 바이트 배열의 크기가 달라진다.
        - EUC-KR은 getBytes()와 마찬가지로 알파벳은 1바이트, 한글은 2바이트로 변환
        - UTF-8은 알파벳은 1바이트, 한글은 3바이트로 변환
    - `getBytes(Charset charset)` 메소드는 잘못된 잘못된 문자셋을 매개값으로 줄 경우
        - `java.io.UnsupportedEncodingException` 예외가 발생하므로 예외 처리가 필요
- 바이트 배열을 다시 문자열로 변환(디코딩)할 때에는 어떤 문자셋으로 인코딩된 바이트 배열이냐에 따라서 디코딩 방법이 다르다.
    - 단순하게 `String(byte[] bytes)` 생성자를 이용해서 디코딩하면 시스템의 기본 문자셋을 이용
    - 시스템 기본 문자셋과 다른 문자셋으로 인코딩된 바이트 배열일 경우 다음 String 생성자를 이용해서 디코딩해야 한다.
        
        ```java
        String str = new String(byte[] bytes, String charsetName);
        ```
        
- ex) 바이트 배열로 변환
    - 문자열을 바이트 배열로 인코딩하고 길이를 출력
    - 다시 String 생성자를 이용해서 문자열로 디코딩
    - StringGetBytesEx.java
    
    ```java
    import java.io.UnsupportedEncodingException;
    
    public class StringGetBytesEx {
    
    	public static void main(String[] args) {
    		String str = "안녕하세요";
    		
    		// 기본 문자셋으로 인코딩과 디코딩
    		byte[] bytes1 = str.getBytes();
    		System.out.println("bytes1.length : " + bytes1.length);
    		String str1 = new String(bytes1);
    		System.out.println("bytes1 -> String : " + str1);
    		
    		try {
    			// EUC-KR을 이용해서 인코딩 및 디코딩
    			byte[] bytes2 = str.getBytes("EUC-KR");
    			System.out.println("bytes2.length : " + bytes2.length);
    			String str2 = new String(bytes2, "EUC-KR");
    			System.out.println("bytes2 -> String : " + str2);
    			
    			// UTF-8을 이용해서 인코딩 및 디코딩
    			byte[] bytes3 = str.getBytes("UTF-8");
    			System.out.println("bytes3.length : " + bytes3.length);
    			String str3 = new String(bytes3, "UTF-8");
    			System.out.println("bytes3 -> String : " + str3);
    			
    		} catch(UnsupportedEncodingException e) {
    			e.printStackTrace();
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/바이트_배열로_변환(getBytes())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판