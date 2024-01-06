---
title: "[Java] 제네릭 타입(class<T>, interface<T>)"
description: ""
date: "2022-10-27T22:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 제네릭 타입은 타입을 파라미터로 가지는 클래스와 인터페이스를 말한다.
- 클래스 또는 인터페이스 이름 뒤에 “`< >`” 부호가 붙고, 사이에 타입 파라미터가 위치
    - 아래 코드에서 타입 파라미터의 이름은 `T`
    
    ```java
    public class 클래스명<T> { ... }
    public interface 인터페이스명<T> { ... }
    ```
    
- 타입 파라미터는 변수명과 동일한 규칙에 따라 작성할 수 있다.
    - 일반적으로 대문자 알파벳 한 글자로 표현
- 제네릭 타입을 실제 코드에서 타입 파라미터에 구체적인 타입을 지정해야 한다.
    
    ### 타입 파라미터를 사용하는 이유
    
    ---
    
    - Box 클래스(비제네릭 타입 이용)
        - Box.java
    
    ```java
    public class Box {
    	private Object object;
    	public void set(Object object) { this.object = object; }
    	public Object get() { return object; }
    }
    ```
    
    - Box 클래스의 필드 타입이 Object인데, Object 타입으로 선언한 이유는 필드에 모든 종류의 객체를 저장하고자 하기 때문이다.
    - Object 클래스는 모든 자바 클랫의 최상위 부모 클래스이다.
        - 자식 객체는 부모 타입에 대입할 수 있다는 성질 때문에 모든 자바 객체는 Object 타입으로 자동 타입 변환되어 저장
        
        ```java
        Object object = 자바의 모든 객체;
        ```
        
    - `set()` 메소드는 매개 변수 타입으로 Object를 사용함으로써 매개값으로 자바의 모든 객체를 받을 수 있게 했다.
        - 받은 매개값을 Object 필드에 저장
    - `get()` 메소드는 Object 필드에 저장된 객체를 Object 타입으로 리턴
        - 만약 필드에 저장된 원래 타입의 객체를 얻으려면 강제 타입 변환을 해야 한다.
        
        ```java
        Box box = new Box();
        
        // String 타입을 Object 타입으로 자동 타입 변환해서 저장
        box.set("hello");
        
        // Object 타입을 String 타입으로 강제 타입 변환해서 얻음
        String str = (String) box.get();
        ```
        
    - Apple 클래스
        - Apple.java
        
        ```java
        public class Apple{
        }
        ```
        
    - 비제네릭 타입 이용
        - BoxEx.java
        
        ```java
        public class BoxEx {
        
        	public static void main(String[] args) {
        		Box box = new Box();
        		box.set("ABC");                   // String -> Object (자동 타입 변환)
        		String name = (String) box.get(); // Object -> String (강제 타입 변환)
        		
        		box.set(new Apple());             // Apple -> Object (자동 타입 변환)
        		Apple apple = (Apple) box.get();  // Object -> Apple (강제 타입 변환) 
        	}
        }
        ```
        
        - Object 타입을 사용하면 모든 종류의 자바 객체를 저장할 수 있다는 장점이 존재
            - 저장할 때 타입 변환이 발생
            - 읽어올 때 타입 변환이 발생
            - 이러한 타입 변환이 빈번해지면 전체 프로그램 성능에 좋지 못한 결과를 가져올 수 있다.
            - 모든 종류의 객체를 저장하면서 타입 변화이 발생하지 않도록 하는 방법
                
                → 제네릭
                
        
        ---
        
    - Box 클래스(제네릭 이용)
        - Box.java
        
        ```java
        public class Box<T>{
        	private T t;
        	public T get() { return t; }
        	public void set(T t) { this.t = t; }
        }
        ```
        
        - 타입 파라미터 T를 사용해서 Object 타입을 모두 T로 대체
        - T는 Box  클래스로 객체를 생성할 때 구체적인 타입으로 변경된다.
            - Box 객체를 생성했다고 가정
            
            ```java
            Box<String> box = new Box<String>();
            ```
            
            - 타입 파라미터 T는 String 타입으로 변경되어 Box 클래스의 내부는 다음과 같이 자동으로 재구성된다.
            
            ```java
            public class Box<String> {
            	private String t;
            	public void set(String t) { this.t = t; }
            	public String get() { return t; }
            }
            ```
            
            - 필드 타입이 String으로 변경
            - `set()` 메소드도 String 타입만 매개값으로 받을 수 있게 변경
            - 다음 코드를 보면 저장할 때와 읽어올 때 전혀 타입 변환이 발생하지 않는다.
            
            ```java
            Box<String> box = new Box<String>();
            box.set("hello");
            String str = box.get();
            ```
            
        - Box 객체를 생성했다고 가정
            - Integer는 int 값에 대한 객체 타입으로 자바에서 제공하는 표준 API
            
            ```java
            Box<Integer> box = new Box<Integer>();
            ```
            
            - 파라미터 T는 Integer 타입으로 변경되어 Box 클래스는 내부적으로 다음과 같이 재구성된다.
            
            ```java
            public class Box<Integer> {
            	private Integer t;
            	public void set(Integer t) { this.t = t }
            	public Integer get() { return t; } 
            }
            ```
            
            - 필드 타입이 Integer로 변경
            - `set()` 메소드도 Integer 타입만 매개값으로 받을수 있게 변경
            - `get()` 메소드는 Integer 타입으로 리턴하도록 변경
            - 다음 코드를 보면 저장할 때와 읽어올 대 전혀 타입 변환이 발생하지 않는다.
            
            ```java
            Box<Integer> box = new Box<Integer>();
            box.set(6);            // 자동 Boxing
            int value = box.get(); // 자동 UnBoxing
            ```
            
        
        ### 이와 같이 제네릭은 클래스를 설계할 때 구체적인 타입을 명시하지 않는다.
        
        ### 타입 파라미터로 대체했다가 실제 클래스가 사용될 때 구체적인 타입을 지정함으로써 타입 변환을 최소화
        
        - 제네릭 타입 이용
            - BoxEx.java
            
            ```java
            public class BoxEx {
            
            	public static void main(String[] args) {
            		Box<String> box1 = new Box<String>();
            		box1.set("ABC");                 
            		String name = box1.get();
            		
            		Box<Integer> box2 = new Box<Integer>();
            		box2.set(6);             
            		int value = box2.get();   
            
            	}
            }
            ```
            
    
---
    
## References
    
- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판