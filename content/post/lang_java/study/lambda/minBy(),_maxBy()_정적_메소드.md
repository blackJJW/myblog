---
title: "[Java] 람다식, minBy(), maxBy() 정적 메소드"
description: ""
date: "2022-11-05T14:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `BinaryOperator<T>` 함수적 인터페이스는 `minBy()`와 `maxBy()` 정적 메소드를 제공
    - 이 두 메소드는 매개값으로 제공되는 Comparator를 이용해서 최대 T와 최소 T를 얻는 `BinaryOperator<T>`를 리턴
    
    | 리턴 타입 | 정적 메소드 |
    | --- | --- |
    | BinaryOperator<T> | minBy(Comparator<? super T> comparator) |
    | BinaryOperator<T> | maxBy(Comparator<? super T> comparator) |
- `Comparator<T>`는 다음과 같이 선언된 함수적 인터페이스
    - o1과 o2를 비교하여 값을 리턴하는 `compare()` 메소드가 선언
        - o1이 작으면 음수를 리턴
        - o1과 o2가 동일하면 0을 리턴
        - o1이 크면 양수를 리턴
    
    ```java
    @FunctionalInterface
    public interface Comparator<T> {
    	public int compare(T o1, T o2);
    }
    ```
    
- `Comparator<T>`를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    
    ```java
    (o1, o2) -> { ...; return int값; }
    ```
    
    - 만약 o1과 o2가 int 타입이라면 다음과 같이 `Integer.compare(int, int)` 메소드를 이용 가능
    - `Integer.compare()`는 첫 번째 매개값이 두 번째 매개값보다 작으면 음수, 같으면 0, 크면 양수를 리턴
    
    ```java
    (o1, o2) -> Integer.compare(o1, o2);
    ```
    
- ex) `minBy()`, `maxBy()` 정적 메소드
    - 두 과일의 값을 비교해서 값이 낮거나 높은 과일을 얻어낸다.
    - OperatorMinByMaxByEx.java
    
    ```java
    import java.util.function.BinaryOperator;
    
    public class OperatorMinByMaxByEx {
    
    	public static void main(String[] args) {
    		BinaryOperator<Fruit> binaryOperator;
    		Fruit fruit;
    		
    		binaryOperator = BinaryOperator.minBy(
    				(f1, f2) -> Integer.compare(f1.price, f2.price)
    				);
    		fruit = binaryOperator.apply(
    				new Fruit("strawberry", 6000), 
    				new Fruit("water melon", 10000)
    				);
    		System.out.println(fruit.name);
    		
    		binaryOperator = BinaryOperator.maxBy(
    				(f1, f2) -> Integer.compare(f1.price, f2.price)
    				);
    		fruit = binaryOperator.apply(
    				new Fruit("strawberry", 6000), 
    				new Fruit("water melon", 10000)
    				);
    		System.out.println(fruit.name);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/minBy(),_maxBy()_정적_메소드/Untitled.png)
    
- ex) Fruit.java
    
    ```java
    public class Fruit {
    	public String name;
    	public int price;
    	
    	public Fruit(String name, int price) {
    		this.name = name;
    		this.price = price;
    	}
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판