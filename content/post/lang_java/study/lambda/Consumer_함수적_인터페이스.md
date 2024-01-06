---
title: "[Java] 람다식, Consumer 함수적 인터페이스"
description: ""
date: "2022-10-31T15:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Consumer 함수적 인터페이스의 특징은 리턴값이 없는 `accept()` 메소드를 가지고 있다.
    - `accept()` 메소드는 다지 매개값을 소비하는 역할을 수행
        - 소비한다는 말은 사용만 할 뿐 리턴값이 없다는 의미
    
    ![Untitled](/images/lang_java/lambda/Consumer_함수적_인터페이스/Untitled.png)
    
- 매개 변수의 타입과 수에 따른 Consumer
    
    
    | 인터페이스명 | 추상 메소드 | 설명 |
    | --- | --- | --- |
    | Consumer<T> | void accept(T t) | 객체 T를 받아 소비 |
    | BiConsumer<T, U> | void accept(T t, U u) | 객체 T와 U를 받아 소비 |
    | DoubleConsumer | void accept(double value) | double 값을 받아 소비 |
    | IntConsumer | void accept(int value) | int 값을 받아 소비 |
    | LongConsumer | void accept(long value) | long 값을 받아 소비 |
    | ObjDoubleConsumer<T> | void accept(T t, double value) | 객체 T와 double 값을 받아 소비 |
    | ObjIntConsumer<T> | void accept(T t, int value) | 객체 T와 int 값을 받아 소비 |
    | ObjLongConsumer<T> | void accept(T t, long value) | 객체 T와 long 값을 받아 소비 |
- `Consumer<T>` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `accept()` 메소드는 매개값으로 T 객체 하나를 가지므로 람다식도 한 개의 매개 변수를 사용
    - 타입 파라미터 T에 String이 대입되었기 때문에 람다식 t 매개 변수 타입은 String이 된다.
    
    ```java
    Consumer<String> consumer = t -> { t를 소비하는 실행문; };
    ```
    
- `BiConsumer<T, U>` 인터페이스를 타겟으로 하는 람다식은 다음과 같이 작성 가능
    - `accept()` 메소드는 매개값으로 T와 U 두 개의 객체를 람다식도 두 개의 매개 변수를 사용
    - 타입 파라미터 T와 U에 String이 대입되었기 때문에 람다식의 t와 u 매개 변수 타입은 각각 String이 된다.
    
    ```java
    BiConsumer<String, String> consumer = (t, u) -> { t와 u를 소비하는 실행문; }
    ```
    
- `DoubleConsumer` 인터페이스를 타겟으로 하는 람다식은 다음과 같이 작성 가능
    - `accept()` 메소드는 매개값으로 double 하나를 가지므로 람다식도 한 개의 매개 변수를 사용
    - d는 고정적으로 double 타입이 된다.
    
    ```java
    DoubleConsumer consumer = d -> { d를 소비하는 실행문; }
    ```
    
- `ObjIntConsumer<T>` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `accept()` 메소드는 매개값으로 T 객체와 int 값 두 개를 가지기 때문에 람다식도 두 개의 매개 변수를 사용
    - T가 String 타입이므로 람다식의 t 매개 변수 타입은 String
    - i는 고정적으로 int 타입
    
    ```java
    ObjIntConsumer<String> consumer = (t, i) -> { t와 i를 소비하는 실행문; }
    ```
    
- ex) Consumer 함수적 인터페이스
    - ConsumerEx.java
    
    ```java
    import java.util.function.BiConsumer;
    import java.util.function.Consumer;
    import java.util.function.DoubleConsumer;
    import java.util.function.ObjIntConsumer;
    
    public class ConsumerEx {
    
    	public static void main(String[] args) {
    		Consumer<String> consumer = t -> System.out.println(t + "8");
    		consumer.accept("java");
    		
    		BiConsumer<String, String> bigConsumer = (t, u) -> System.out.println(t + u);
    		bigConsumer.accept("Java", "8");
    		
    		DoubleConsumer doubleConsumer = d -> System.out.println("Java" + d);
    		doubleConsumer.accept(8.0);
    		
    		ObjIntConsumer<String> objIntConsumer = (t, i) -> System.out.println(t + i);
    		objIntConsumer.accept("Java", 8);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/Consumer_함수적_인터페이스/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판