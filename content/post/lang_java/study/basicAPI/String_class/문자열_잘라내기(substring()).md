---
title: "[Java] String 클래스, String 메소드, 문자열 잘라내기(substring())"
description: ""
date: "2022-09-25T21:45:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `substring()` 메소드는 주어진 인덱스에서 문자열을 추출
- `substring()` 메소드는 매개값의 수에 따라 두 가지 형태로 사용
    - `substring(int beginIndex, int endIndex)`는 주어진 시작과 끝 인덱스 사이의 문자열을 추출
    - `substring(int beginIndex)`는 주어진 인덱스부터 끝까지 문자열을 추출
    
    ```java
    String ssn = "920123-1234567";
    String firstNum = ssn.substring(0, 6);
    String secondNum = ssn.substring(7);
    ```
    
    - firstNum 변수값 : “920123”
    - secondNum 변수값 : “1234567”
    
    ![Untitled](/images/lang_java/basicAPI/문자열_잘라내기(substring())/Untitled.png)
    
    - `ssn.substring(0, 6)`은 인덱스 0(포함) ~ 6(제외) 사이의 문자열을 추출
    - `ssn.substring(7)`은 인덱스 7부터의 문자열을 추출
- ex) 문자열 추출하기
    - StringSubstringEx.java
    
    ```java
    public class StringSubstringEx {
    
    	public static void main(String[] args) {
    		String ssn = "920123-1234567";
    		
    		String firstNum = ssn.substring(0, 6);
    		System.out.println(firstNum);
    		
    		String secondNum = ssn.substring(7);
    		System.out.println(secondNum);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_잘라내기(substring())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판