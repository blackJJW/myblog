---
title: "[Java] 강제 타입 변환(Casting)"
description: ""
date: "2022-08-09T20:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 부모 타입을 자식 타입으로 변환하는 것
    - 모든 부모 타입을 자식 클래스 타입으로 강제 변환 가능한 것은 아니다.
    - 자식 타입이 부모 탕비으로 자동 변환한 후, 다시 자식 타입으로 변환할 때 가제 타입 변환을 사용 가능
    
    ![Untitled](/images/lang_java/inheritance/강제_타입_변환(Casting)/Untitled.png)
    
- 자식 타입이 부모 타입으로 자동 변환할 경우
    - 부모 타입에 선언된 필드와 메소드만 사용 가능하다는 제약 사항이 따른다.
    - 만약 자식 타입에 선언된 필드와 메소드를 꼭 사용해야할 경우
        - 강제 타입 변환을 해서 다시 자식 타입으로 변환한 다음 자식 타입의 필드와 메소드를 사용하면 된다.
    
    ![Untitled](/images/lang_java/inheritance/강제_타입_변환(Casting)/Untitled%201.png)
    
    - field2 필드와 `method3()` 메소드는 Child 타입에만 선언되어 있으므로 Parent 타입으로 자동 타입 변환하면 사용할 수 없다.
    - field2 필드와 `method3()` 메소드를 사용하고 싶다면 다시 Child 타입으로 강제 타입 변환을 해야 한다.
- ex)
    - Parent.java
        - 부모 클래스
        
        ```java
        public class Parent {
        	public String field1;
        	
        	public void method1() {
        		System.out.println("Parent-method1()");
        	}
        	
        	public void method2() {
        		System.out.println("Parent-method2()");
        	}
        }
        ```
        
    - Child.java
        - 자식 클래스
        
        ```java
        public class Child extends Parent{
        	public String field2;
        	
        	public void method3() {
        		System.out.println("Child-method3()");
        	}
        }
        ```
        
    - ChildEx.java
        - 강제 타입 변환(캐스팅)
        
        ```java
        public class ChildEx {
        
        	public static void main(String[] args) {
        		Parent parent = new Child();
        		parent.field1 = "data1";
        		parent.method1();
        		parent.method2();
        		/*
        		 * parent.field2 = "data2"; //(불가능)
        		 * parent.method3();        //(불가능)
        		 */
        		
        		Child child = (Child) parent;
        		child.field2 = "yyy"; //(가능)
        		child.method3();      //(가능)
        
        	}
        
        }
        ```
        
        ![Untitled](/images/lang_java/inheritance/강제_타입_변환(Casting)/Untitled%202.png)
        
        ![Untitled](/images/lang_java/inheritance/강제_타입_변환(Casting)/Untitled%203.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판