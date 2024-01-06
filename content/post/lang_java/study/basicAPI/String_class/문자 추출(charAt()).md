---
title: "[Java] String 클래스, String 메소드, 문자 추출(charAt())"
description: ""
date: "2022-09-24T20:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `charAt()` 메소드는 매개값으로 주어진 인덱스의 문자를 리턴
    
    ```java
    String subject = "자바 프로그래밍";
    char charValue = subject.charAt(3);
    ```
    
    - 인덱스란 0에서부터 “문자열길이-1”까지의 번호를 말한다.
    
    ![Untitled](/images/lang_java/basicAPI/문자_추출(charAt())/Untitled.png)
    
    - `charAt(3)`은 3인텍스 위치에 있는 문자를 의미
- ex) 주민등록번호에서 남자와 여자를 구분하는 방법
    - 주민등록번호에서 인덱스 7번 문자를 읽어 남자와 여자를 구분
    - StringCharAtEx.java
    
    ```java
    public class StringCharAtEx {
    
    	public static void main(String[] args) {
    		String ssn = "920123-1234567";
    		char sex = ssn.charAt(7);
    		switch(sex) {
    		case '1':
    		case '3':
    			System.out.println("male");
    			break;
    		case '2':
    		case '4':
    			System.out.println("female");
    			break;
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자_추출(charAt())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판