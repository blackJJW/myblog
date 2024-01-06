---
title: "[Java] String 클래스, String 메소드, 문자열 비교(equals())"
description: ""
date: "2022-09-24T20:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 기본 타입(byte, char, short, int, long, float, double, boolean) 변수의 값을 비교할 때에는 == 연산자를 이용한다.
    - 문자열을 비교할 때에는 == 연산자를 사용하면 원하지 않는 결과가 나올 수 있다.
    
    ```java
    String strVar1 = new String("ABC");
    String strVar2 = "ABC";
    String strVar3 = "ABC";
    ```
    
    - 자바는 문자열 리터럴이 동일하다면 동일한 String 객체를 참조한다.
        - strVar2와 strVar3는 동일한 String 객체를 참조
        - strVar1은 new 연산자로 생성된 다른 String 객체를 참조
        
        ![Untitled](/images/lang_java/basicAPI/문자열_비교(equals())/Untitled.png)
        
    - 이 경우 변수 strVar1과 strVar2의 == 연산은 false를 산출하고 strVar2와 strVar3의 == 연산은 true를 산출
        - == 연산자는 각 변수에 저장된 번지를 비교하기 때문
        
        ```java
        strVar1 == strVar2 // false
        strVar2 == strVar3 // true
        ```
        
- String 객체의 문자열만을 비교하고 싶다면 == 연산자 대신에 `equals()` 메소드를 사용해야 한다.
    
    ```java
    strVar1.equals(strVar2) // true
    strVar2.equals(strVar3) // true
    ```
    
- 원래 `equals()`는 Object의 번지 비교 메소드이지만, String 클래스가 오버라이딩해서 문자열을 비교하도록 변경했다.
- ex) 문자열 비교
    - StringEqualsEx.java
    
    ```java
    public class StringEqualsEx {
    
    	public static void main(String[] args) {
    		String strVar1 = new String("ABC");
    		String strVar2 = "ABC";
    		
    		if(strVar1 == strVar2) {
    			System.out.println("같은 String 객체를 참조");
    		} else {
    			System.out.println("다른 String 객체를 참조");
    		}
    		
    		if(strVar1.equals(strVar2)) {
    			System.out.println("같은 문자열을 가짐");
    		} else {
    			System.out.println("다른 문자열을 가짐");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_비교(equals())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판