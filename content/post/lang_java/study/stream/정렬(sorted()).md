---
title: "[Java] 스트림, 정렬(sorted())"
description: ""
date: "2022-11-27T15:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 스트림은 요소가 최종 처리되기 전에 중간 단계에서 요소를 정렬해서 최종 처리 순서를 변경 가능
    
    ### 요소를 정렬하는 메소드
    
    | 리턴 타입 | 메소드(매개 변수) | 설명 |
    | --- | --- | --- |
    | Stream<T> | sorted() | 객체를 Comparable 구현 방법에 따라 정렬 |
    | Stream<T> | sorted(Comparator<T>) | 객체를 주어진 Comparator에 따라 정렬 |
    | DoubleStream | sorted() | double 요소를 오름차순으로 정렬 |
    | IntStream | sorted() | int 요소를 오름차순으로 정렬 |
    | LongStream | sorted() | long 요소를 오름차순으로 정렬 |
- 객체 요소일 경우에는 Comparable을 구현하지 않으면 `sorted()` 메소드를 호출했을 때 `ClassCastException`이 발생하기 때문에 Comparable을 구현한 요소에서만 `sorted()` 메소드를 호출해야 한다.
- ex) 정렬 가능한 클래스
    - 점수를 기준으로 Student 요소를 오름차순으로 정렬하기 위해 Comparable을 구현
    - Student.java
    
    ```java
    public class Student implements Comparable<Student>{
    	private String name;
    	private int score;
    	
    	public Student(String name, int score) {
    		this.name = name;
    		this.score = score;
    	}
    	
    	public String getName() { return name; }
    	public int getScore() { return score; }
    	
    	@Override
    	public int compareTo(Student o) {
    		return Integer.compare(score, o.score);
    		/* score < o.score : 음수 리턴
    		 * score == o.score : 0 리턴
    		 * score > o.score : 양수 리턴
    		 * */
    	}
    }
    ```
    
- 객체 요소가 Comparable을 구현한 상태에서 기본 비교(Comparable) 방법으로 정렬하고 싶다면 다음 세 가지 방법 중 하나를 선택해서 `sorted()`를 호출하면 된다.
    
    ```java
    sorted();
    sorted( (a, b) -> a.compareTo(b) );
    sorted( Comparator.naturalOrder() );
    ```
    
- 만약 객체 요소가 Comaprable을 구현하고 있지만, 기본 비교 방법과 정반대 방법으로 정렬하고 싶다면 다음과 같이 `sorted()`를 호출
    
    ```java
    sorted( (a, b) -> b.compareTo(a) );
    sorted( Comparator.reverseOrder() );
    ```
    
- 객체 요소가 Comparable를 구현하지 않았다면 Comparator를 매개값으로 갖는 `sorted()` 메소드를 사용
    - Comparator는 함수적 인터페이스이므로 다음과 같이 람다식으로 매개값을 작성 가능
    
    ```java
    sorted( (a, b) -> {...} )
    ```
    
    - 중괄호 `{}` 안에는 a와 b를 비교해서 a가 작으면 음수, 같으면 0, a가 크면 양수를 리턴하는 코드를 작성하면 된다.
- ex) 정렬
    - 숫자 요소일 경우에는 오름차순으로 정렬한 후 출력
    - Student 요소일 경우에는 Student의 기본 비교(Comparable) 방법을 이용해서 점수를 기준으로 오름차순으로 정렬한 후 출력
    - Comparator를 제공해서 점수를 기준으로 내림차순으로 정렬한 후 출력
    - SortingEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.Comparator;
    import java.util.List;
    import java.util.stream.IntStream;
    
    public class SortingEx {
    
    	public static void main(String[] args) {
    		// 숫자 요소일 경우
    		IntStream intStream = Arrays.stream(new int[] {5, 3, 2, 1, 4});
    		intStream
    			.sorted() // 숫자를 오름차순으로 정렬
    			.forEach(n -> System.out.print(n + ", "));
    		System.out.println();
    		
    		// 객체 요소일 경우
    		List<Student> studentList = Arrays.asList(
    				new Student("ABC", 30),
    				new Student("DEF", 10),
    				new Student("GHI", 20)
    				);
    		
    		studentList.stream()
    			.sorted() // 정수를 기준으로 오름차순으로 Student 정렬
    			.forEach(s -> System.out.print(s.getScore() + ", "));
    		System.out.println();
    		
    		studentList.stream()
    			.sorted( Comparator.reverseOrder() ) // 정수를 기준으로 내림차순으로 Student 정렬
    			.forEach(s -> System.out.print(s.getScore() + ", "));
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/정렬(sorted())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판