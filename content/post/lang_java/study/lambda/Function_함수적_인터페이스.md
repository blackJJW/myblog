---
title: "[Java] 람다식, Function 함수적 인터페이스"
description: ""
date: "2022-11-02T21:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Function 함수적 인터페이스의 특징은 매개값과 리턴값이 있는 `applyXXX()` 메소드를 가지고 있다.
    - 이 메소드들은 매개값을 리턴값으로 매핑(타입 변환)하는 역할을 수행
    
    ![Untitled](/images/lang_java/lambda/Function_함수적_인터페이스/Untitled.png)
    
- 매개 변수 타입과 리턴 타입에 따른 Function 함수적 인터페이스
    
    
    | 인터페이스명 | 추상 메소드 | 설명 |
    | --- | --- | --- |
    | Function<T, R> | R apply(T t) | 객체 T를 객체 R로 매핑 |
    | BiFunction<T, U, R> | R apply(T t, U u) | 객체 T와 U를 객체 R로 매핑 |
    | DoubleFunction<R> | R apply(double value) | double을 객체 R로 매핑 |
    | IntFunction<R> | R apply(int value) | int를 객체 R로 매핑 |
    | IntToDoubleFunction | double applyAsDouble(int value) | int를 double로 매핑 |
    | IntToLongFunction | long applyAsLong(int value) | int를 long으로 매핑 |
    | LongToDoubleFunction | double applyAsDouble(long value) | long을 double로 매핑 |
    | LongToIntFunction | int applyAsInt(long value) | long을 int로 매핑 |
    | ToDoubleBiFunction<T, U> | double applyAsDouble(T t, U u) | 객체 T와 U를 double로 매핑 |
    | ToDoubleFunction<T> | double applyAsDouble(T t) | 객체 T를 double로 매핑 |
    | ToIntBiFunction<T, U> | int applyAsInt(T t, U u) | 객체 T와 U를 int로 매핑 |
    | ToIntFunction<T> | int applyAsInt(T t) | 객체 T를 int로 매핑 |
    | ToLongBiFunction<T, U> | long applyAsLong(T t, U u) | 객체 T와 U를 long으로 매핑 |
    | ToLongFunction<T> | long applyAsLong(T t) | 객체 T를 long으로 매핑 |
- `Function<T, R>` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `apply()` 메소드는 매개값으로 T 객체 하나를 가지므로 람다식도 한 개의 매개 변수를 사용
    - `apply()` 메소드의 리턴 타입이 R이므로 람다식 중괄호 {}의 리턴값은 R 객체가 된다.
    - ex) Student 객체를 학생 이름(String)으로 매핑
        - T가 Student 타입이고 R이 String 타입
            - t 매개 변수 타입은 Student
            - 람다식 중괄호 `{}`는 String을 리턴
        - `t.getName()`은 Student 객체의 `getName()` 메소드를 호출해서 학생 이름(String)을 얻는다.
        - return 문만 있르 경우 중괄호 `{}`와 return 문 생략 가능
        
        ```java
        Function<Student, String> function = t -> { return t.getName(); }
        
        또는
        
        Function<Student, String> fucntion = t -> t.getName();
        ```
        
- `ToIntFunction<T>` 인터페이스를 타겟 타입으로 하는 람다식은 다음과 같이 작성 가능
    - `applyAsInt()` 메소드는 매개값으로 T 객체 하나를 가지므로 람다식도 한 개의 매개 변수를 사용
    - `applyAsInt()` 메소드의 리턴 타입이 int이므로 람다식 중괄호 `{}`의 리턴값은 int가 된다.
    - ex) Student 객체를 학생 점수(int)로 매핑
        - T가 Student 타입이므로 t 매개 변수 타입은 Student가 된다.
        - `t.getScore()`는 Student 객체의 `getScore()` 메소드를 호출해서 학생 점수(int)를 얻는다.
        
        ```java
        ToIntFunction<Student> function = t -> { return t.getScore(); }
        
        또는
        
        ToIntFunction<Student> function = t -> t.getScore();
        ```
        
- ex) Function 함수적 인터페이스
    - List에 저장된 학생 객체를 하나씩 꺼내서 이름과 점수를 출력
    - FunctionEx1의 `printString()` 메소드는 `Function<Student, String>` 매개 변수를 가지고 있다.
    - `printInt()` 메소드는 `ToIntFunction<Student>` 매개 변수를 가지고 있으므로 이 메소드들을 호출할 때 매개값으로 람다식을 사용 가능
    - FunctionEx1.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.function.Function;
    import java.util.function.ToIntFunction;
    
    public class FunctionEx1 {
    	private static List<Student> list = Arrays.asList(
    			new Student("ABC", 90, 96),
    			new Student("DEF", 95, 93)
    			);
    	
    	public static void printString( Function<Student, String> function) {
    		for(Student student : list) { // list에 저장된 항목 수만큼 루핑
    			System.out.println(function.apply(student) + " ");
    			                  // 람다식 실행
    		}
    		System.out.println();
    	}
    	
    	public static void printInt( ToIntFunction<Student> function ) {
    		for(Student student : list) { // list에 저장된 항목 수만큼 루핑
    			System.out.println(function.applyAsInt(student) + " ");
    			                 // 람다식 실행
    		}
    		System.out.println();
    	}
    			
    	public static void main(String[] args) {
    		System.out.println("[ 학생 이름 ]");
    		printString( t -> t.getName() );
    		
    		System.out.println("[ 영어 점수 ]");
    		printInt( t -> t.getEnglishScore() );
    		
    		System.out.println("[ 수학 점수 ]");
    		printInt( t -> t.getMathScore() );
    	}
    }
    ```
    
    - Student.java
        - Student 클래스
    
    ```java
    public class Student {
    	private String name;
    	private int englishScore;
    	private int mathScore;
    	
    	public Student(String name, int englishScore, int mathScore) {
    		this.name = name;
    		this.englishScore = englishScore;
    		this.mathScore = mathScore;
    	}
    	
    	public String getName() { return name; }
    	public int getEnglishScore() { return englishScore; }
    	public int getMathScore() { return mathScore; }
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/Function_함수적_인터페이스/Untitled%201.png)
    
- ex) Function 함수적 인터페이스
    - List에 저장된 학생 객체를 하나씩 꺼내서 영어 점수와 수학 점수의 평균값을 산출
    - FunctionEx2의 `avg()` 메소드는 `ToIntFunction<Student>` 매개 변수를 가지고 있다.
    - `avg()` 메소드를 호출할 때 매개값으로 람다식을 사용 가능
    - 람다식은 `getEnglishScore()`와 `getMathScore()`를 호출해서 영어 점수와 수학 점수로 Student 객체를 매핑(변환)시킨다.
    - FunctionEx2.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    import java.util.function.ToIntFunction;
    
    public class FunctionEx2 {
    	private static List<Student> list = Arrays.asList(
    			new Student("ABC", 90, 96),
    			new Student("DEF", 95, 93)
    			);
    	
    	public static double avg( ToIntFunction<Student> function ) {
    		int sum = 0;
    		for(Student student : list) {
    			sum += function.applyAsInt(student);
    			     // 람다식 실행
    		}
    		double avg = (double) sum / list.size();
    		return avg;
    	}
    
    	public static void main(String[] args) {
    		double englishAvg = avg( s -> s.getEnglishScore() );
    		System.out.println("영어 평균 점수 : " + englishAvg);
    		
    		double mathAvg = avg( s -> s.getMathScore() );
    		System.out.println("수학 평균 점수 : " + mathAvg);
    
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/Function_함수적_인터페이스/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판