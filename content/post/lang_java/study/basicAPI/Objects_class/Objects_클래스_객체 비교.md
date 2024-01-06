---
title: "[Java] Objects 클래스, 객체 비교(compare(T a, T b, Comparator<T>c))"
description: ""
date: "2022-09-20T22:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `java.util.Objects` 클래스는 객체 비교, 해시코드 생성, null 여부, 객체 문자열 리턴 등의 연산을 수행하는 정적 메소드들로 구성된 Object의 유틸리티 클래스
- Objects 클래스가 가지고 있는 정적 메소드
    
    ![Untitled](/images/lang_java/basicAPI/Objects_클래스_객체_비교/Untitled.png)
    

# 객체 비교(compare(T a, T b, Comparator<T>c))

- `Objects.compare(T a, T b, Comparator<T>c)` 메소드는 두 객체를 비교자(Comparator)로 비교해서 int 값을 리턴
- `java.util.Comparator<T>`는 제네릭 인터페이스 타입
    - 두 객체를 비교하는 `compare(T a, T b)` 메소드가 정의되어 있다.
    - `T`가 비교할 객체 타입
- compare() 메소드의 리턴 타입은 int
    - a가 b보다 작으면 음수 리턴
    - 같으면 0 리턴
    - a가 b보다 크면 양수 리턴
    - 위와 같이 리턴하도록 구현 클래스를 만들어야 한다.
    
    ```java
    public interface Comparator<T> {
       int compare(T a, T b);
    }
    ```
    
- ex) 학생 번호 비교자
    - 학생 객체에서 학생 번호로 비교하는 StudentComparator 구현 클래스 작성
    - a의 sno가 작으면 -1, 같으면 0, 크면 1을 리턴
    - StudentComparator.java
    
    ```java
    public class StudentComparator implements Comparator<Student>{
    	@Override
    	public int compare(Student a, Student b) {
    		if(a.sno < b.sno) return -1;
    		else if(a.studentNo == b.sno) return 0;
    		else return 1;
    		// 간단하게 다음과 같이 코드 대체 가능
    		// return Integer.compare(a.sno, b.sno);
    	}
    
    }
    ```
    
- ex) 비교자 사용
    - 세 개의 학생 객체를 StudentComparator로 비교해서 결과를 리턴
    - CompareEx.java
    
    ```java
    import java.util.Comparator;
    import java.util.Objects;
    
    public class CompareEx {
    
    	public static void main(String[] args) {
    		Student s1 = new Student(1);
    		Student s2 = new Student(1);
    		Student s3 = new Student(2);
    		
    		int result = Objects.compare(s1, s2, new StudentComparator());
    		System.out.println(result);
    		result = Objects.compare(s1, s3, new StudentComparator());
    		System.out.println(result);
    
    	}
    	
    	static class Student{
    		int sno;
    		Student(int sno){
    			this.sno = sno;
    		}
    	}
    	
    	static class StudentComparator implements Comparator<Student>{
    		@Override
    		public int compare(Student o1, Student o2) {
    			return Integer.compare(o1.sno, o2.sno);
    		}
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/Objects_클래스_객체_비교/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판