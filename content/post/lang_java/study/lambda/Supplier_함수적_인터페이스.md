---
title: "[Java] 람다식, Supplier 함수적 인터페이스"
description: ""
date: "2022-10-31T16:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Supplier 함수적 인터페이스의 특징은 매개 변수가 없고 리턴값이 있는 `getXXX()` 메소드를 가지고 있다.
    - 이 메소드들은 실행 후 호출한 곳으로 데이터를 리턴하는 역할을 수행
    
    ![Untitled](/images/lang_java/lambda/Supplier_함수적_인터페이스/Untitled.png)
    
- 리턴 타입에 따른 Supplier 함수적 인터페이스
    
    
    | 인터페이스명 | 추상 메소드 | 설명 |
    | --- | --- | --- |
    | Supplier<T> | T get() | T 객체를 리턴 |
    | BooleanSupplier | boolean getAsBoolean() | boolean 값을 리턴 |
    | DoubleSupplier | double getAsDouble() | double 값을 리턴 |
    | IntSupplier | int getAsInt() | int 값을 리턴 |
    | LongSupplier | long getAsLong() | long 값을 리턴 |
- `Supplier<T>` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `get()` 메소드가 매개값을 가지지 않으므로 람다식도 `()`를 사용
    - 람다식의 중괄호 `{}`는 반드식 한 개의 T 객체를 리턴하도록 해야 한다.
    - T가 String 타입이므로 람다식의 중괄호 `{}`는 문자열을 리턴하도록 해야한다.
    
    ```java
    Supplier<String> supplier = () -> { ...; return "문자열"; }
    ```
    
- `IntSupplier` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `getAsInt()` 메소드가 매개값을 가지지 않으므로 람다식도 `()`를 사용
    - 람다식의 중괄호 `{}`는 반드시 int 값을 리턴하도록 해야 한다.
    
    ```java
    IntSupplier supplier = () -> { ...; return int값; }
    ```
    
- ex) Supplier 함수적 인터페이스
    - 주사위의 숫자를 랜덤하게 공급하는 IntSupplier 인터페이스를 타겟 타입으로 하는 람다식
    - SupplierEx.java
    
    ```java
    import java.util.function.IntSupplier;
    
    public class SupplierEx {
    
    	public static void main(String[] args) {
    		// 람다식
    		IntSupplier intSupplier = () -> {
    			int num = (int) (Math.random() * 6) + 1;
    			return num;
    		};
    		
    		int num = intSupplier.getAsInt();
    		System.out.println("눈의 수 : " + num);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/Supplier_함수적_인터페이스/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판