---
title: "[Java] String 클래스, String 메소드, 문자열 길이(length())"
description: ""
date: "2022-09-25T21:15:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `length()` 메소드는 문자열의 길이(문자의 수)를 리턴
    
    ```java
    String subject = "자바 프로그래밍";
    int length = subject.length();
    ```
    
    - length 변수에는 8이 저장
        - subject 객체의 문자열의 길이는 공백을 포함해서 8개
    
    ![Untitled](/images/lang_java/basicAPI/문자열_길이(length())/Untitled.png)
    
- ex) 문자열의 문자 수 얻기
    - StringLengthEx.java
    
    ```java
    public class StringLengthEx {
    
    	public static void main(String[] args) {
    		String ssn = "9201231234567";
    		
    		int length = ssn.length();
    		if(length == 13) {
    			System.out.println("주민번호 자리수 맞음");
    		} else {
    			System.out.println("주민번호 자리수 틀림");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_길이(length())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판