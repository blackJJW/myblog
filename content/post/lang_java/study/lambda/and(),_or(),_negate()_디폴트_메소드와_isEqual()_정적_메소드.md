---
title: "[Java] 람다식, and(), or(), negate() 디폴트 메소드와 isEqual() 정적 메소드"
description: ""
date: "2022-11-05T13:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Predicate 종류의 함수적 인터페이스는 `and()`, `or()`, `negate()` 디폴트 메소드를 가지고 있다.
    - 이 메소드들은 각각 논리 연산자인 `&&`, `||`, `!`과 대응
- `and()` 메소드 : Predicate가 모두 true를 리턴하면 최종적으로 true를 리턴하는 Predicate를 생성
- `or()` 메소드 : 두 Predicate 중 하나만 true를 리턴하더라도 최종적으로 true를 리턴하는 Predicate를 생성
- `negate()` 메소드 : 원래 Predicate의 결과가 true이면 false로, false이면 true를 리턴하는 새로운 Predicate를 생성
    
    ### Predicate 함수적 인터페이스
    
    | 함수적 인터페이스 | and() | or()  | negate()  |
    | --- |:-----:|:---------:|:---:|
    | Predicate<T> | ○ |   ○   |     ○     |
    | BiPredicate<T, U> | ○ |   ○   |     ○     |
    | DoublePredicate | ○ |   ○   |     ○     |
    | IntPredicate | ○ |   ○   |     ○     |
    | LongPredicate | ○ |   ○   |     ○     |
- ex) Predicate 간의 논리 연산
    - 2의 배수와 3의 배수를 조사하는 두 Predicate를 논리 연산한 새로운 Predicate를 생성
    - PredicateAndOrNegateEx.java
    
    ```java
    import java.util.function.IntPredicate;
    
    public class PredicateAndOrNegateEx {
    
    	public static void main(String[] args) {
    		// 2의 배수 검사
    		IntPredicate predicateA = a -> a % 2 == 0;
    		
    		// 3의 배수 검사
    		IntPredicate predicateB = (a) -> a % 3 == 0;
    		
    		IntPredicate predicateAB;
    		boolean result;
    		
    		// and()
    		predicateAB = predicateA.and(predicateB);
    		result = predicateAB.test(9);
    		System.out.println("9는 2와 3의 배수? " + result);
    		
    		// or()
    		predicateAB = predicateA.or(predicateB);
    		result = predicateAB.test(9);
    		System.out.println("9는 2 또는 3의 배수? " + result);
    		
    		// negate()
    		predicateAB = predicateA.negate();
    		result = predicateAB.test(9);
    		System.out.println("9는 홀수? " + result);
    
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/and(),_or(),_negate()_디폴트_메소드와_isEqual()/Untitled.png)
    
- `Predicate<T>` 함수적 인터페이스는 `and()`, `or()`, `negate()` 디폴트 메소드 이외에 `isEqual()` 정적 메소드를 추가로 제공
- `isEqual()` 메소드는 `test()` 매개값인 sourceObject와 `isEqual()`의 매개값인 targetObject를 `java.util.Objects` 클래스의 `equals()`의 매개값으로 제공
    - `Objects.equals(sourceObject, targetObject)`의 리턴값을 얻어 새로운 `Predicate<T>`를 생성
    
    ```java
    Predicate<Object> predicate = Predicate.isEqual(targetObject);
    boolean result = predicate.test(sourceObject);
                                // targetObject와 sourceObject를 동등비교
                                // Objects.equals(sourceObject, targetObject) 실행
    ```
    
- `Objects.equals(sourceObject, targetObject)`는 다음과 같은 리턴값을 제공
    
    
    | sourceObject | targetObject | 리턴값 |
    | :---: | :---: | :---: |
    | null | null | true |
    | not null | null | false |
    | null | not null | false |
    | not null | not null | sourceObject.equals(targetObject)의 리턴값 |
- ex) `isEqual()` 정적 메소드
    - 두 문자열을 비교하기 위해 Predicate의 `isEqual()` 정적 메소드를 사용
    - PredicateIsEqualEx.java
    
    ```java
    import java.util.function.Predicate;
    
    public class PredicateIsEqualEx {
    
    	public static void main(String[] args) {
    		Predicate<String> predicate;
    		
    		predicate = Predicate.isEqual(null);
    		System.out.println("null, null : " + predicate.test(null));
    		
    		predicate = Predicate.isEqual("Java8");
    		System.out.println("null, Java8 : " + predicate.test(null));
    		
    		predicate = Predicate.isEqual(null);
    		System.out.println("Java8, null : " + predicate.test("Java8"));
    		
    		predicate = Predicate.isEqual("Java8");
    		System.out.println("Java8, Java8 : " + predicate.test("Java8"));
    		
    		predicate = Predicate.isEqual("Java8");
    		System.out.println("Java7, Java8 : " + predicate.test("Java7"));
    
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/and(),_or(),_negate()_디폴트_메소드와_isEqual()/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판