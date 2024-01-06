---
title: "[Java] 스트림, 커스텀 집계(reduce())"
description: ""
date: "2022-11-30T23:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 프로그램화해서 다양한 집계 결과물을 만들 수 있도록 `reduce()` 메소드 제공
    - 인터페이스 : Stream
        
        
        | 리턴 타입 | 메소드(매개 변수) |
        | --- | --- |
        | Optional<T> | reduce(BinaryOperator<T> accumulator) |
        | T | reduce(T identity, BinaryOperator<T> accumulator) |
    - 인터페이스 : IntStream
        
        
        | 리턴 타입 | 메소드(매개 변수) |
        | --- | --- |
        | OptionalInt | reduce(IntBinaryOperator op) |
        | int | reduce(int identity, IntBinaryOperator op) |
    - 인터페이스 : LongStream
        
        
        | 리턴 타입 | 메소드(매개 변수) |
        | --- | --- |
        | OptionalLong | reduce(LongBinaryOperator op) |
        | long | reduce(long identity, LongBinaryOperator op) |
    - 인터페이스 : DoubleStream
        
        
        | 리턴 타입 | 메소드(매개 변수) |
        | --- | --- |
        | OptionalDouble | reduce(DoubleBinaryOperator op) |
        | double | reduce(double identity, DoubleBinaryOperator op) |
- 각 인터페이스에는 매개 타입으로 `XXXOperator`, 리턴 타입으로 `OptionalXXX`, `int`, `long`, `double`을 가지는 `reduce()` 메소드가 오버로딩되어 있다.
- 스트림에 요소가 전혀 없을 경우 디폴트 값이 identity 매개값이 리턴
- `XXXOperator` 매개값은 집계 처리를 위한 람다식을 대입
    - ex) 학생들의 성적 총점은 학생 스트림에서 점수 스트림으로 매핑해서 다음과 같이 얻을 수 있다.
        
        ```java
        int sum = studentList.stream()
        	.map(Student :: getScore)
        	.reduce((a, b) -> a + b)
        	.get();
        ```
        
        - 위 코드는 스트림에 요소가 없을 경우 NoSuchElementException이 발생
        
        ```java
        int sum = studentList.stream()
        	.map(Student :: getScore)
        	.reduce(0, (a, b) -> a + b);
        ```
        
        - 위 코드는 디폴트 값(identity)인 0을 리턴
        - 스트림에 요소가 있을 경우, 두 코드 모두 동일한 결과를 산출
- ex) `reduce()` 메소드 이용
    - ReductionEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    
    public class ReductionEx {
    
    	public static void main(String[] args) {
    		List<Student> studentList = Arrays.asList(
    				new Student("ABC", 92),
    				new Student("DEF", 95),
    				new Student("GHI", 88)
    				);
    		
    		// sum() 이용
    		int sum1 = studentList.stream()
    				.mapToInt(Student :: getScore)
    				.sum();
    		
    		// reduce(BinaryOperator<Integer> ac) 이용
    		int sum2 = studentList.stream()
    				.map(Student :: getScore)
    				.reduce((a, b) -> a+b)
    				.get();
    		
    		// reduce(int identity, IntBinaryOperator op) 이용
    		int sum3 = studentList.stream()
    				.map(Student :: getScore)
    				.reduce(0, (a, b) -> a+b);
    		
    		System.out.println("sum1 : " + sum1);
    		System.out.println("sum2 : " + sum2);
    		System.out.println("sum3 : " + sum3);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/커스텀_집계(reduce())/Untitled.png)
    
- ex) 학생 클래스
    - Student.java
    
    ```java
    public class Student {
    	private String name;
    	private int score;
    	
    	public Student(String name, int score) {
    		this.name = name;
    		this.score = score;
    	}
    	
    	public String getName() { return name; }
    	public int getScore() { return score; }
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판