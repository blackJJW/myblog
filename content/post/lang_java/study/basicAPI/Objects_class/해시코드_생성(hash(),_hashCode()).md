---
title: "[Java] Objects 클래스, 해시코드 생성(hash(), hashCode())"
description: ""
date: "2022-09-20T23:45:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `Objects.hash(Object … values)` 메소드는 매개값으로 주어진 값들을 이용해서 해시 코드를 생성하는 역할을 수행
    - 주어진 매개값들로 배열을 생성하고 `Arrays.hashCode(Object[])`를 호출해서 해시코드를 얻고 이 값을 리턴
- 이 메소드는 클래스가 `hashCode()`를 재정의할 때 리턴값을 생성하기 위해 사용하면 좋다.
- 클래스가 여러 가지 필드를 가지고 있을 때
    - 이 필드로부터 해시코드를 생성하게 되면 동일한 필드값을 가지는 객체는 동일한 해시코드를 가질 수 있다.
    
    ```java
    @Override
    public int hashCode(){
    	return Objects.hash(field1, field2, field3);
    }
    ```
    
    - `Objects.hashCode(Object o)`는 매개값으로 주어진 객체의 해시코드를 리턴하기 때문에 `o.hashCode()`의 리턴값과 동일
    - 차이점은 매개값이 null이면 0을 리턴
- ex) 해시코드 생성
    - Student 객체의 해시코드를 생성하기 위해 Student의 필드인 sno(학생 번호)와 name(학생 이름)을 매개값으로 해서 `Objects.hash()` 메소드를 호출
    - 학생 번화와 이름이 동일하다면 같은 해시 코드를 얻을 수 있다는 것을 보여줌
    - HashCodeEx.java
    
    ```java
    import java.util.Objects;
    
    public class HashCodeEx {
    
    	public static void main(String[] args) {
    		Student s1 = new Student(1, "ABC");
    		Student s2 = new Student(1, "ABC");
    		System.out.println(s1.hashCode());
    		System.out.println( Objects.hashCode(s2) );
    
    	}
    	
    	static class Student{
    		int sno;
    		String name;
    		Student(int sno, String name){
    			this.sno = sno;
    			this.name = name;
    		}
    		@Override
    		public int hashCode() {
    			return Objects.hash(sno, name);
    		}
    		
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/해시코드_생성(hash(),_hashCode())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판