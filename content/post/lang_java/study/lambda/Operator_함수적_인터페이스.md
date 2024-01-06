---
title: "[Java] 람다식, Operator 함수적 인터페이스"
description: ""
date: "2022-11-03T21:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Operator 함수적 인터페이스는 Function과 동일하게 매개 변수와 리턴값이 있는 `applyXXX()` 메소드를 가지고 있다.
    - 하지만 이 메소드들은 매개값을 리턴값으로 매핑(타입 변환)하는 역할보다는 매개값을 이용해서 연산을 수행한 후 동일한 타입으로 리턴값을 제공하는 역할을 수행
    
    ![Untitled](/images/lang_java/lambda/Operator_함수적_인터페이스/Untitled.png)
    
- 매개 변수 타입과 수에 따른 Operator 함수적 인터페이스
    
    
    | 인터페이스명 | 추상 메소드 | 설명 |
    | --- | --- | --- |
    | BinaryOperator<T> | T apply(T t, T t) | T와 T를 연산한 후 T 리턴 |
    | UnaryOperator<T> | T apply(T t) | T를 연산한 후 T 리턴 |
    | DoubleBinaryOperator | double applyAsDouble(double, double) | 두 개의 double 연산 |
    | DoubleUnaryOperator | double applyAsDouble(double) | 한 개의 double 연산 |
    | IntBinaryOperator | int applyAsInt(int, int) | 두 개의 int 연산 |
    | IntUnaryOperator | int applyAsInt(int) | 한 개의 int 연산 |
    | LongBinaryOperator | long applyAsLong(long, long) | 두 개의 long 연산 |
    | LongBinaryOperaotr | long applyAsLong(long) | 한 개의 long 연산 |
- `IntBinaryOperator` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `applyAsInt()` 메소드는 매개값으로 두 개의 int를 가지므로 람다식도 두 개의 int 매개 변수 a와 b를 사용
    - `applyAsInt()` 메소드의 리턴 타입이 int이므로 람다식의 중괄호 `{}`의 리턴값은 int가 된다.
    - 다음 코드는 두 개의 int를 연산해서 결과값으로 int를 리턴
    
    ```java
    IntBinaryOperator operator = (a, b) -> { ...;, return int값; }
    ```
    
- ex) Operator 함수적 인터페이스
    - int[] 배열에서 최대값과 최소값을 얻는다.
    - `maxOrMin()` 메소드는 `IntBinaryOperator` 매개 변수를 가지고 있다.
        - 따라서 `maxOrMin()` 메소드를 호출할 때 람다식 사용 가능
    - OperatorEx.java
    
    ```java
    import java.util.function.IntBinaryOperator;
    
    public class OperatorEx {
    	private static int[] scores = { 92, 95, 87 };
    	
    	public static int maxOrMin( IntBinaryOperator operator ) {
    		int result = scores[0];
    		for(int score : scores) {
    			result = operator.applyAsInt(result, score);
    			               // 람다식 실행
    		}
    		return result;
    	}
    	
    	public static void main(String[] args) {
    		// 최대값 얻기
    		int max = maxOrMin(
    				// 람다식(큰값 리턴)
    				(a, b) -> {
    					if(a >= b) return a;
    					else return b;
    				}
    				);
    		System.out.println("최대값 : " + max);
    		
    		// 최소값 얻기
    		int min = maxOrMin(
    				// 람다식(작은값 리턴)
    				(a, b) -> {
    					if(a <= b) return a;
    					else return b;
    				}
    				);
    		System.out.println("최소값 : " + min);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/Operator_함수적_인터페이스/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판