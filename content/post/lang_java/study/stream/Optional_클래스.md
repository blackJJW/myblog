---
title: "[Java] 스트림, 기본 집계, Optional 클래스"
description: ""
date: "2022-11-29T23:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `Optional`, `OptionalDouble`, `OptionalInt`, `OptionalLong` 클래스
- Optional 클래스는 단순히 집계 값만 저장하는 것이 아니다.
    - 집계 값이 존재하지 않을 경우 디폴트 값을 설정 가능
    - 집계 값을 처리하는 Consumer도 등록 가능
    
    ### Optional 클래스가 제공하는 메소드
    
    ![Untitled](/images/lang_java/stream/Optional_클래스/Untitled.png)
    
- 컬렉션의 요소는 동적으로 추가되는 경우가 많다.
    - 만약 컬렉션의 요소가 추가되지 않아 저장된 요소가 없을 경우 다음 코드는 어떻게 될까?
        
        ```java
        List<Integer> list = new ArrayList<>();
        double avg = list.stream()
        	.mapToInt(Integer :: intValue)
        	.average()
        	.getAsDouble();
        System.out.println("평균 : " + avg);
        ```
        
        - 요소가 없기 때문에 평균값도 있을 수 없다.
            - `NoSuchElementException`예외가 발생
            
            ![Untitled](/images/lang_java/stream/Optional_클래스/Untitled%201.png)
            
    - 요소가 없을 경우 예외를 피하는 세 가지 방법
        1. Optional 객체를 얻어 `isPresent()` 메소드로 평균값 여부를 확인하는 것
            - `isPresent()` 메소드가 true를 리턴할 때만 `getAsDouble()` 메소드로 평균값을 얻으면 된다.
            
            ```java
            OptionalDouble optional = list.stream()
            	.mapToInt(Integer :: intValue)
            	.average();
            if(optional.isPresent()) {
            	System.out.println("평균 : " + optional.getAsDouble());
            } else {
            	System.out.println("평균 : 0.0");
            }
            ```
            
        2. `orElse()` 메소드로 디폴트 값을 정한다.
            - 평균값을 구할 수 없는 경우에는 `orElse()`의 매개값이 디폴트 값이 된다.
            
            ```java
            double avg = list.stream()
            	.mapToInt(Integer :: intValue)
            	.average()
            	.orElse(0.0);
            System.out.println("평균 : + avg)
            ```
            
        3. `ifPresent()` 메소드로 평균값이 있를 경우에만 값을 이용하는 람다식을 실행
            
            ```java
            list.stream()
            	.mapToInt(Integer :: intValue)
            	.average()
            	.ifPresent(a -> System.out.println("평균 : " + a));
            ```
            
- ex) 집계
    - OptionalEx.java
    
    ```java
    import java.util.ArrayList;
    import java.util.List;
    import java.util.OptionalDouble;
    
    public class OptionalEx {
    
    	public static void main(String[] args) {
    		List<Integer> list = new ArrayList<>();
    		
    		/*// 예외 발생 : java.util.NoSuchElementException
    		double avg = list.stream()
    				.mapToInt(Integer :: intValue)
    				.average()
    				.getAsDouble();
    		*/
    		
    		// 방법 1
    		OptionalDouble optional = list.stream()
    				.mapToInt(Integer :: intValue)
    				.average();
    		if(optional.isPresent()) {
    			System.out.println("방법1_평균 : " + optional.getAsDouble());
    		} else {
    			System.out.println("방법1_평균 : 0.0");
    		}
    		
    		// 방법 2
    		double avg = list.stream()
    				.mapToInt(Integer :: intValue)
    				.average()
    				.orElse(0.0);
    		System.out.println("방법2_평균 : " + avg);
    		
    		// 방법 3
    		list.stream()
    			.mapToInt(Integer :: intValue)
    			.average()
    			.ifPresent(a -> System.out.println("방법3_평균 : " + a));
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/Optional_클래스/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판