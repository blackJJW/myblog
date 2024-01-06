---
title: "[Java] 스트림, 루핑(peek(), forEach())"
description: ""
date: "2022-11-28T21:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 루핑은(looping)은 요소 전체를 반복하는 것을 말함
    - 루핑은 메소드에는 `peek()`, `forEach()`가 존재
        - 이 두 메소드는 루핑한다는 기능에서는 동일, 동작 방식은 다름
        - `peek()`는 중간 처리 메소드
        - `forEach()`는 최종 처리 메소드
- `peek()`는 중간 처리 단계엣 전체 요소를 루핑하면서 추가적인 작업을 하기 위해 사용
- 최종 처리 메소드가 실행되지 않으면 지연되기 때문에 반드시 최종 처리 메소드가 호출되어야 동작
- ex) 필터링 후 어떤 요소만 남았는지 확인하기 위해 다음과 같이 peek()를 마지막에 호출할 경우
    - 스트림은 전혀 동작하지 않는다.
    
    ```java
    intStream
    	.filter( a -> a % 2 == 0 )
    	.peek( a -> System.out.println(a) )
    ```
    
    - 요소 처리의 최종 단계가 합을 구하는 것이라면, `peek()` 메소드 호출 후 `sum()`을 호출해야만 `peek()`가 정상적으로 동작
        
        ```java
        intStream
        	.filter( a -> a % 2 == 0 )
        	.peek( a -> System.out.println(a) )
        	.sum()
        ```
        
- `forEach()`는 최종 처리 메소드이기 때문에 파이프라인 마지막에 루핑하면서 요소를 하나씩 처리
    - `forEach(`)는 요소를 소비하는 최종 처리 메소드이므로 이후에 `sum()`과 같은 다른 최종 메소드를 호출하면 안된다.
- ex) 루핑
    - LoopingEx.java
    
    ```java
    import java.util.Arrays;
    
    public class LoopingEx {
    
    	public static void main(String[] args) {
    		int[] intArr = { 1, 2, 3, 4, 5 };
    		
    		System.out.println("[ peek()를 마지막에 호출한 경우 ]");
    		Arrays.stream(intArr)
    			.filter( a -> a % 2 == 0 )
    			.peek( n -> System.out.println(n) ); // 동작하지 않음
    		
    		System.out.println("[ 최종 처리 메소드를 마지막에 호출한 경우 ]");
    		int total = Arrays.stream(intArr)
    			.filter( a -> a % 2 == 0 )
    			.peek( n -> System.out.println(n) ) // 동작함
    			.sum();   // 최종 메소드
    		System.out.println("총합 : " + total);
    		
    		System.out.println("[ forEach()를 마지막에 호출한 경우 ]");
    		Arrays.stream(intArr)
    			.filter( a -> a % 2 == 0 )
    			.forEach( n -> System.out.println(n) ); // 최종 메소드로 동작
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/루핑(peek(),_forEach())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판