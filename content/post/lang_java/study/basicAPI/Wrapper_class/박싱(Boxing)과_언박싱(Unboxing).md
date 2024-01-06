---
title: "[Java] Wrapper(포장) 클래스, 박싱(Boxing)과 언박싱(Unboxing)"
description: ""
date: "2022-09-29T21:23:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 기본 타입의 값을 포장 객체로 만드는 과정을 박싱(Boxing)
- 포장 객체에서 시본 타입의 값을 얻어내는 과정을 언박싱(Unboxing)
    
    ### 8개의 기본 타입의 값을 박싱하는 방법
    
    - 간단하게 포장 클래스의 생성자 매개값으로 기본 타입의 값 또는 문자열을 넘겨주면 된다.
    
    | 기본 타입의 값을 줄 경우 | 문자열을 줄 경우 |
    | --- | --- |
    | Byte obj = new Byte(10); | Byte obj = new Byte(”10”); |
    | Character obj = new Character(”A”); | 없음 |
    | Short obj = new Short(100); | Short obj = new Short(”100”); |
    | Integer obj = new Integer(1000); | Integer obj = new Integer(”1000”); |
    | Long obj = new Long(10000); | Long obj = new Long(”10000”); |
    | Float obj = new Float(2.5F); | Float obj = new Float(”2.5F”); |
    | Double obj = new Double(3.5); | Double obj = new Double(”3.5”); |
    | Boolean obj = new Boolean(true); | Boolean obj = new Boolean(”true”); |
    - 생성자를 이용하지 않아도 각 포장 클래스마다 가지고 있는 정적 `valueOf()` 메소드 사용 가능
        
        ```java
        Integer obj = Integer.valueOf(1000);
        Integer obj = Integer.valueOf("1000");
        ```
        
    
    ### 박싱된 포장 객체에서 다시 기본 탕비의 값을 얻어내는 방법(언박싱)
    
    - 각 포장 클래스마다 가지고 있는 “`기본타입명+Value()`” 메소드를 호출
    - 기본 타입의 값을 이용
        
        
        | 타입 | 메소드호출 |
        | --- | --- |
        | byte | num = obj.byteValue(); |
        | char | ch = obj.charValue(): |
        | short | num = obj.shortValue(); |
        | int | num = obj.intValue(); |
        | long | num = obj.longValue(); |
        | float | num = obj.floatValue(); |
        | double | num = obj.doubleValue(); |
        | boolean | bool = obj.booleanValue(); |
- ex) 기본 타입의 값을 박싱하고 언박싱하기
    - BoxingUnBoxingEx.java
    
    ```java
    public class BoxingUnBoxingEx {
    	public static void main(String[] args) {
    		// Boxing
    		Integer obj1 = new Integer(100);
    		Integer obj2 = new Integer("200");
    		Integer obj3 = Integer.valueOf("300");
    		
    		// Unboxing
    		int value1 = obj1.intValue();
    		int value2 = obj2.intValue();
    		int value3 = obj3.intValue();
    		
    		System.out.println(value1);
    		System.out.println(value2);
    		System.out.println(value3);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/박싱(Boxing)과_언박싱(Unboxing)/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판