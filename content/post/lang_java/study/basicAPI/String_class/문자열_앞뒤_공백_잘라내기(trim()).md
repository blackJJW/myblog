---
title: "[Java] String 클래스, String 메소드, 문자열 앞뒤 공백 잘라내기(trim())"
description: ""
date: "2022-09-25T22:10:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `trim()` 메소드는 문자열의 앞뒤 공백을 제거한 새로운 문자열을 생성하고 리턴
    
    ```java
    String oldStr = "   자바 프로그래밍   ";
    String newStr = oldStr.trim();
    ```
    
    - newStr 변수는 새로 생성된 “자바 프로그래밍” 문자열을 참조
    - `trim()` 메소드는 앞뒤의 공백만 제거할 뿐 중간의 공백은 제거하지 않는다.
    - `trim()` 메소드를 사용한다고 해서 원래 문자열의 공백이 제거되는 것은 아니다.
    
    ![Untitled](/images/lang_java/basicAPI/문자열_앞뒤_공백_잘라내기(trim())/Untitled.png)
    
- ex) 앞뒤 공백 제거
    - StringTrimEx.java
    
    ```java
    public class StringTrimEx {
    
    	public static void main(String[] args) {
    		String tel1 = "   02";
    		String tel2 = "123   ";
    		String tel3 = "   1234   ";
    		
    		String tel = tel1.trim() + tel2.trim() + tel3.trim();
    		System.out.println(tel);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_앞뒤_공백_잘라내기(trim())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판