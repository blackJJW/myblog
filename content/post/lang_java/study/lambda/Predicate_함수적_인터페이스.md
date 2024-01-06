---
title: "[Java] 람다식, Predicate 함수적 인터페이스"
description: ""
date: "2022-11-03T22:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Predicate 함수적 인터페이스는 매객 변수와 boolean 리턴값이 있는 `testXXX()` 메소드를 가지고 있다.
    - 이 메소드들은 매개값을 조사해서 true 또는 false를 리턴하는 역할을 수행
    
    ![Untitled](/images/lang_java/lambda/Predicate_함수적_인터페이스/Untitled.png)
    
- 매개 변수 타입과 수에 따른 Predicate 함수적 인터페이스
    
    
    | 인터페이스명 | 추상 메소드 | 설명 |
    | --- | --- | --- |
    | Predicate<T> | boolean test(T t) | 객체 T를 조사 |
    | BiPredicate<T, U> | boolean test(T t, U u) | 객체 T와 U를 비교 조사 |
    | DoublePredicate | boolean test(double value) | 객체 T와 U를 비교 조사 |
    | IntPredicate | boolean test(int value) | int 값을 조사 |
    | LongPredicate | boolean test(long value) | long 값을 조사 |
- `Predicate<T>` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `test()` 메소드는 매개값으로 T 객체 하나를 가지므로 람다식도 한 개의 매개 변수를 사용
    - `test()` 메소드의 리턴 타입이 boolean이므로 람다식 중괄호 `{}`의 리턴값은 boolean이 된다.
    - ex) String의 `equals()` 메소드를 이용해 남학생만 true를 리턴
        - T가 Student 타입이므로 t 매개 변수 타입은 Student가 된다.
        - `t.getSex()`는 객체의 `getSex()` 메소드를 호출해서 “남자” 또는 “여자”를 얻는다.
    
    ```java
    Predicate<Student> predicate = t -> { return t.getSex().equals("남자"); }
    
    또는
    
    Predicate<Student> predicate = t -> t.getSex().equals("남자");
    ```
    
- ex) Predicate 함수적 인터페이스
    - List에 저장된 남자 또는 여자 학생들의 평균 점수를 리턴
    - `avg()` 메소드는 `Predicate<Student>` 매개 변수를 가지고 있다.
        - 따라서 `avg()` 메소드를 호출할 때 매개값으로 람다식을 사용 가능
    - PredicateEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.function.Predicate;
    
    public class PredicateEx {
    	private static List<Student> list = Arrays.asList(
    			new Student("ABC", "남자", 90),
    			new Student("DEF", "여자", 90),
    			new Student("GHI", "남자", 95),
    			new Student("JKL", "여자", 92)
    			);
    	
    	public static double avg( Predicate<Student> predicate ) {
    		int count = 0, sum = 0;
    		for(Student student : list) {
    			if( predicate.test(student) ) {
    				count++;
    				sum += student.getScore();
    			}
    		}
    		return (double) sum / count;
    	}
    	
    	public static void main(String[] args) {
    		double maleAvg = avg( t -> t.getSex().equals("남자"));
    		System.out.println("남자 평균 점수 : " + maleAvg);
    		
    		double femaleAvg = avg( t -> t.getSex().equals("여자"));
    		System.out.println("여자 평균 점수 : " + femaleAvg);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/Predicate_함수적_인터페이스/Untitled%201.png)
    
- ex) Student 클래스
    - Student.java
    
    ```java
    public class Student {
    	private String name;
    	private String sex;
    	private int score;
    	
    	public Student(String name, String sex, int score) {
    		this.name = name;
    		this.sex = sex;
    		this.score = score;
    	}
    	
    	public String getSex() { return sex; }
    	public int getScore() { return score; }
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판