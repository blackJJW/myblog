---
title: "[Java] Object 클래스, 객체 문자 정보(toString())"
description: ""
date: "2022-09-18T22:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Object 클래스의 toString() 메소드는 객체의 문자 정보를 리턴
    - 객체의 문자 정보 : 객체를 문자열로 표현한 값
    - 기본적으로 “`클래스명@16진수해시코드`”로 구성된 문자 정보를 리턴
    
    ```java
    Object obj = new Object();
    System.out.println( obj.toString() );
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_문자_정보(toString())/Untitled.png)
    
- Object의 `toString()` 메소드의 리턴값은 자바 애플리케이션에서는 별 값어치가 없는 정보
    - Object 하위 클래스는 `toString()` 메소드를 오버라이딩하여 간결하고 유익한 정보를 리턴하도록 되어 있다.
    - ex) java.util 패키지의 Date 클래스는 `toString()` 메소드를 오버라이딩하여 현재 시스템의 날짜와 시간 정보를 리턴
        - String 클래스는 `toString()` 메소드를 오버라이딩해서 저장하고 있는 문자열을 리턴
- ex) 객체의 문자 정보(`toString()` 메소드)
    - Object 클래스와 Date 클래스의 `toString()` 메소드의 리턴값을 출력
    - ToStringEx.java
    
    ```java
    import java.util.Date;
    
    public class ToStringEx {
    
    	public static void main(String[] args) {
    		Object obj1 = new Object();
    		Date obj2 = new Date();
    		
    		System.out.println(obj1.toString());
    		System.out.println(obj2.toString());
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_문자_정보(toString())/Untitled%201.png)
    
    - toString() 메소드를 재정의하여 좀 더 유용한 정보를 리턴할 수 있도록 할 수 있다.
    - SmartPhone.java
    
    ```java
    public class SmartPhone {
    	private String company;
    	private String os;
    	
    	public SmartPhone(String company, String os) {
    		this.company = company;
    		this.os = os;
    	}
    	
    	@Override
    	public String toString() { // toString 재정의
    		return company + ", " + os;
    	}
    }
    ```
    
    - SmartPhoneEx.java
    
    ```java
    public class SmartPhoneEx {
    
    	public static void main(String[] args) {
    		SmartPhone myPhone = new SmartPhone("Google", "Android");
    		
    		String strObj = myPhone.toString();
    		System.out.println(strObj);
    		
    		System.out.println(myPhone);
    		// myPhone.toString()을 자동 호출해서 리턴값을 얻은 후 호출
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_문자_정보(toString())/Untitled%202.png)
    

- 콘솔에 출력하기 위해 `System.out.println()` 메소드를 사용
    - 이 메소드의 매개값은 콘솔에 출력할 내용
    - 매개값이 기본 타입(byte, short, int, long, float, double, boolean)일 경우, 해당 값을 그대로 출력
    - 매개값으로 객체를 줄 경우, 객체의 `toString()` 메소드를 호출해서 리턴값을 받아 출력

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판