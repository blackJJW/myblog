---
title: "[Java] 스트림, 매핑, asDoubleStream(), asLongStream(), boxed() 메소드"
description: ""
date: "2022-11-27T14:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `asDoubledStream()` 메소드는 IntStream의 int 요소 또는 LongStream의 long 요소를 double 요소로 타입 변환해서 DoubleStream을 생성
- `asLongStream()` 메소드는 IntStream의 int 요소를 long 요소로 타입 변환해서 LongStream을 생성
- `boxed()` 메소드는 int, long, double 요소를 Integer, Long, Double 요소로 박싱해서 Stream을 생성
    
    ![Untitled](/images/lang_java/stream/asDoubleStream(),_asLongStream(),_boxed()_메소드/Untitled.png)
    
- ex) 다른 요소로 대체
    - `int[]` 배열로부터 IntStream을 얻고 난 다음 int 요소를 double 요소로 타입 변환해서 DoubleStream을 생성
    - int 요소를 Integer 객체로 박싱해서 Stream<Integer>를 생성
    - AsDoubleStreamAndBoxedEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.stream.IntStream;
    
    public class AsDoubleStreamAndBoxedEx {
    
    	public static void main(String[] args) {
    		int[] intArray = { 1, 2, 3, 4, 5 };
    		
    		IntStream intStream = Arrays.stream(intArray);
    		intStream
    			.asDoubleStream() // DoubleStream 생성
    			.forEach(d -> System.out.println(d));
    		
    		System.out.println();
    		
    		intStream = Arrays.stream(intArray);
    		intStream
    			.boxed()
    			.forEach(obj -> System.out.println(obj.intValue()));
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/asDoubleStream(),_asLongStream(),_boxed()_메소드/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판