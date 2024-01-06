---
title: "[Java] 스트림, 매핑, mapXXX() 메소드"
description: ""
date: "2022-11-27T13:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `mapXXX()` 메소드는 요소를 대체하는 요소로 구성된 새로운 스트림을 리턴
- 그림 예시
    - 스트림에서 A 요소는 C 요소로 대체
    - B 요소는 D 요소로 대체
    - C, D 요소를 가지는 새로운 스트림이 생성
    
    ![Untitled](/images/lang_java/stream/mapXXX()_메소드/Untitled.png)
    
    ### `mapXXX()` 메소드의 종류
    
    | 리턴 타입 | 메소드(매개 변수) | 요소 -> 대체 요소 |
    | --- | --- | --- |
    | Stream<R> | map(Function<T, R>) | T -> R |
    | DoubleStream | mapToDouble(ToDoubleFunction<T>) | T -> double |
    | IntStream | mapToInt(ToIntFunction<T>) | T -> int |
    | LongStream | mapToLong(ToLongFunction<T>) | T -> long |
    | DoubleStream | map(DoubleUnaryOperator) | double -> double |
    | IntStream | mapToInt(DoubleToIntFunction) | double -> int |
    | LongStream | mapToLong(DoubleToLongFunction) | double -> long |
    | Stream<U> | mapToObj(DoubleFunction<U>) | double -> U |
    | IntStream | map(IntUnaryOperator) | int -> int |
    | DoubleStream | mapToDouble(IntToDoubleFunction) | int -> double |
    | LongStream | mapToLong(IntToLongFunction) | int -> long |
    | Stream<U> | mapToObj(IntFunction<U>) | int -> U |
    | LongStream | map(LongUnaryOperator) | long -> long |
    | DoubleStream | mapToDouble(LongToDoubleFunction) | long -> double |
    | IntStream | mapToInt(LongToIntFunction) | long -> int |
    | Stream<U> | mapToObj(LongFunction<U>) | long -> U |
- ex) 다른 요소로 대체
    - 학생 List에서 학생의점수를 요소로 하는 새로운 스트림을 생성
    - 점수를 순차적을 콘솔에 출력
    - MapEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    
    public class MapEx {
    
    	public static void main(String[] args) {
    		List<Student> studentList = Arrays.asList(
    				new Student("ABC", 10),
    				new Student("DEF", 20),
    				new Student("GHI", 30)
    				);
    		
    		studentList.stream()
    			.mapToInt(Student :: getScore)
    			.forEach(score -> System.out.println(score));
    
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/mapXXX()_메소드/Untitled%201.png)
    
- ex) 학생 클래스
    - Student.java
    
    ```java
    public class Student {
    	private String name;
    	private int score;
    	
    	public Student (String name, int score) {
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