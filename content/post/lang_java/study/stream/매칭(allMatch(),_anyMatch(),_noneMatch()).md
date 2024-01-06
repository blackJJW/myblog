---
title: "[Java] 스트림, 매칭(allMatch(), anyMatch(), noneMatch())"
description: ""
date: "2022-11-28T22:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 스트림 클래스는 최종 처리 단계에서 요소들이 특정 조건에 만족하는 지 조사할 수 있도록 세 가지 매칭 메소드를 제공
    - `allMatch()` : 모든 요소들이 매개값으로 주어진 Predicate의 조건을 만족하는 지 조사
    - `anyMatch()` : 최소한 한 개의 요소가 매개값으로 주어진 Predicate의 조건을 만족하는 지 조사
    - `noneMatch()` : 모든 요소들이 매개값으로 주어진 Predicate의 조건을 만족하지 않는지 조사
    
    ---
    
    - 리턴 타입 : boolean
        - 제공 인터페이스 : Stream
            - 메소드(매개 변수)
                - `allMatch(Predicate<T> predicate)`
                - `anyMatch(Predicate<T> predicate)`
                - `noneMatch(Predicate<T> predicate)`
        - 제공 인터페이스 : IntStream
            - 메소드(매개 변수)
                - `allMatch(IntPredicate<T> predicate)`
                - `anyMatch(IntPredicate<T> predicate)`
                - `noneMatch(IntPredicate<T> predicate)`
        - 제공 인터페이스 : LongStream
            - 메소드(매개 변수)
                - `allMatch(LongPredicate<T> predicate)`
                - `anyMatch(LongPredicate<T> predicate)`
                - `noneMatch(LongPredicate<T> predicate)`
        - 제공 인터페이스 : DoubleStream
            - 메소드(매개 변수)
                - `allMatch(DoublePredicate<T> predicate)`
                - `anyMatch(DoublePredicate<T> predicate)`
                - `noneMatch(DoublePredicate<T> predicate)`
- ex) 매칭
    - `int[]` 배열로부터 스트림을 생성
    - 모든 요소가 2의 배수인지, 하나라도 3의 배수가 존재하는지, 모든 요소가 3의 배수가 아닌지 조사
    - MatchEx.java
    
    ```java
    import java.util.Arrays;
    
    public class MatchEx {
    
    	public static void main(String[] args) {
    		int[] intArr = { 2, 4, 6 };
    		
    		boolean result = Arrays.stream(intArr)
    				.allMatch(a -> a % 2 == 0);
    		System.out.println("모두 2의 배수인가? : " + result);
    		
    		result = Arrays.stream(intArr)
    				.anyMatch(a -> a % 3 == 0);
    		System.out.println("하나라도 3의 배수가 있는가? : " + result);
    		
    		result = Arrays.stream(intArr)
    				.noneMatch(a -> a % 3 == 0);
    		System.out.println("3의 배수가 없는가? : " + result);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/매칭(allMatch(),_anyMatch(),_noneMatch())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판