---
title: "[Java] String 클래스, String 메소드, 문자열 변환(valueOf())"
description: ""
date: "2022-09-25T22:20:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `valueOf()` 메소드는 기본 타입의 값을 문자열로 변환하는 기능을 가지고 있다.
- String 클래스에서는 매개 변수의 타입별로 `valueOf()` 메소드가 다음과 같이 오버로딩되어 있음
    
    ```java
    static String valueOf(boolean b)
    static String valueOf(char c)
    static String valueOf(int i)
    static String valueOf(long l)
    static String valueOf(double d)
    static String valueOf(float f)
    ```
    
- ex) 기본 타입 값을 문자열로 변환
    - StringValueOfEx.java
    
    ```java
    public class StringValueOfEx {
    
    	public static void main(String[] args) {
    		String str1 = String.valueOf(10);
    		String str2 = String.valueOf(10.5);
    		String str3 = String.valueOf(true);
    		
    		System.out.println(str1);
    		System.out.println(str2);
    		System.out.println(str3);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_변환(valueOf())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판