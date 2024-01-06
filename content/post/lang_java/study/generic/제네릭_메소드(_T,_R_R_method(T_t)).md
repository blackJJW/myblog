---
title: "[Java] 제네릭 메소드(<T, R> R method(T t))"
description: ""
date: "2022-10-28T17:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 제네릭 메소드는 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드
- 제네릭 메소드를 선언하는 방법
    - 리턴 타입 앞에 `<>` 기호를 추가하고 타입 파라미터를 기술
    - 리턴 타입과 매개 타입으로 타입 파라미터를 사용
    
    ```java
    public <타입파라미터, ...> 리턴타입 메소드명(매개변수, ...) { ... }
    ```
    
    - 다음 `boxing()` 제네릭 메소드는 `<>` 기호 안에 타입 파라미터 T를 기술
        - 매개 변수 타입으로 `T`를 사용
        - 리턴 타입으로 제네릭 타입 `Box<T>`
        
        ```java
        public <T> Box<T> boxing(T t) { ... }
        ```
        
- 제네릭 메소드는 두 가지 방법으로 호출 가능
    - 코드에서 타입 파라미터의 구체적인 타입을 명시적으로 지정
    - 컴파일러가 매개값의 타입을 보고 구체적인 타입을 추정
    
    ```java
    리턴타입 변수 = <구체적타입> 메소드명(매개값); // 명시적으로 구체적 타입을 지정
    리턴타입 변수 = 메소드명(매개값);             // 매개값을 보고 구체적 타입을 추정
    ```
    
    - `boxing()` 메소드를 호출하는 코드
        
        ```java
        Box<Integer> box = <Integer>boxing(100);// 타입 파라미터를 명시적으로 Integer로 지정
        Box<Integer> box = boxing(100);         // 타입 파라미터를 Integer로 추정
        ```
        
- ex) 제네릭 메소드
    - Util 클래스에 정적 제네릭 메소드로 `boxing()`을 정의
    - BoxingMethodEx 클래스에서 호출
    - Util.java
    
    ```java
    public class Util {
    	public static <T> Box<T> boxing(T t){
    		Box<T> box = new Box<T>();
    		box.set(t);
    		return box;
    	}
    }
    ```
    
    - BoxingMethodEx.java
        - 제네릭 메소드 호출
    
    ```java
    public class BoxingMethodEx {
    
    	public static void main(String[] args) {
    		Box<Integer> box1 = Util.<Integer>boxing(100);
    		int intValue = box1.get();
    		
    		Box<String> box2 = Util.boxing("ABC");
    		String strValue = box2.get();
    	}
    }
    ```
    
- ex) 제네릭 메소드
    - Util 클래스에 정적 제네릭 메소드로 `compare()`를 정의
    - CompareMethodEx 클래스에서 호출
    - 타입 파라미터는 `K`와 `V`로 선언
        - 제네릭 타입 `Pair`가 `K`와 `V`를 가지고 있기 때문
    - `compare()` 메소드는 두 개의 Pair를 매개값으로 받아 K와 V값이 동일한지 검사하고 boolean 값을 리턴
    - Util.java
    
    ```java
    public class Util {
    	public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2){
    		boolean keyCompare = p1.getKey().equals(p2.getKey());
    		boolean valueCompare = p1.getValue().equals(p2.getValue());
    		return keyCompare && valueCompare;
    	}
    }
    ```
    
    - Pair.java
    
    ```java
    public class Pair<K, V> {
    	private K key;
    	private V value;
    	
    	public Pair(K key, V value) {
    		this.key = key;
    		this.value = value;
    	}
    	
    	public void setKey(K key) { this.key = key; }
    	public void setValue(V value) { this.value = value; }
    	public K getKey() { return key; }
    	public V getValue() { return value; }
    }
    ```
    
    - CompareMethodEx.java
        - 제네릭 메소드 호출
    
    ```java
    public class CompareMethodEx {
    
    	public static void main(String[] args) {
    		Pair<Integer, String> p1 = new Pair<Integer, String>(1, "apple");
    		Pair<Integer, String> p2 = new Pair<Integer, String>(1, "apple");
    		boolean result1 = Util.<Integer, String>compare(p1, p2);
    		                     // 구체적 타입을 명시적으로 지정
    		
    		if(result1) {
    			System.out.println("논리적으로 동등한 객체");
    		} else {
    			System.out.println("논리적으로 동등하지 않은 객체");
    		}
    		
    		Pair<String, String> p3 = new Pair<String, String>("user1", "ABC");
    		Pair<String, String> p4 = new Pair<String, String>("user2", "ABC");
    		boolean result2 = Util.compare(p3, p4);
    		                    // 구체적 타입을 추정
    		
    		if(result2) {
    			System.out.println("논리적으로 동등한 객체");
    		} else {
    			System.out.println("논리적으로 동등하지 않은 객체");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/generic/제네릭_메소드(_T,_R_R_method(T_t))/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판