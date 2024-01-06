---
title: "[Java] 객체 타입 확인(instanceof)"
description: ""
date: "2022-08-10T20:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 강제 타입 변환은 자식 타입이 부모 타입으로 변환되어 있는 상태에서만 가능하기 깨문데 부모타입의 변수가 부모 객체를 참조할 경우 자식 타입으로 변환 불가
    
    ```java
    Parent parent = new Parent();
    Child child = (Child) parent; // 강제 타입 변환 불가
    ```
    

### 부모 변수가 참조하는 객체가 부모 객체인지 자식 객체인지 확인하는 방법

- 어떤 객체가 어떤 클래스의 인스턴스인지 확인하려면 `instanceof` 연산자 사용 가능
- `instanceof` 연산자
    - 매개값의 타입을 조사할 때 주로 사용
    - 좌항 : 객체
    - 우항 : 타입
    
    ```java
    boolean result = 좌항(객체) instanceof 우항(타입)
    ```
    
    - 좌항의 객체가 우항의 인스턴스이면, 즉 우항의 인스턴스이면 true를 출력
        - 그렇지 않으면 false를 출력
- 메소드 내에서 강제 타입 변환이 필요할 경우 반드시 매개값이 어떤 객체인지 `instanceof` 연산자로 확인하고 안전하게 강제 타입 변환을 해야 한다.
    
    ![Untitled](/images/lang_java/inheritance/객체_타입_확인(instanceof)/Untitled.png)
    

---

- 만약 타입을 확인하지 않고 강제 타입 변환을 시도한다면 `ClassCastException` 예외가 발생할 수 있다.
- ex) InstanceofEx 클래스에서 `method1()`과 `method2()`는 모두 Parent 타입의 매개값을 받도록 선언
    - Parent.java
        - 부모 클래스
        
        ```java
        public class Parent {
        
        }
        ```
        
    - Child.java
        - 자식 클래스
        
        ```java
        public class Child extends Parent{
        
        }
        ```
        
    - InstanceofEx.java
        - 객체 타입 확인
        
        ```java
        public class InstanceofEx {
        	public static void method1(Parent parent) {
        		if(parent instanceof Child) {	// Child 타입으로 변환이 가능한지 확인
        			Child child = (Child) parent;
        			System.out.println("method1 - Child로 변환 성공");
        		} else {
        			System.out.println("method1 - Child로 변환되지 않음");
        		}
        	}
        	
        	public static void method2(Parent parent) {
        		Child child = (Child) parent;	// ClassCastException 발생 가능
        		System.out.println("method2 - Child로 변환 성공");
        	}
        	
        	public static void main(String[] args) {
        		Parent parentA = new Child();
        		method1(parentA);	// Child 객체를 
        		method2(parentA);	// 매개값으로 전달
        		
        		Parent parentB = new Parent();
        		method1(parentB);				  // Parent 객체를
        		method2(parentB);	// 예외 발생   // 매개값으로 전달 
        	}
        
        }
        ```
        
        ![Untitled](/images/lang_java/inheritance/객체_타입_확인(instanceof)/Untitled%201.png)
        
        - InstanceofEx 클래스에서 `method1()`과 `method2()`를 호출할 경우
            - Child 객체를 매개값으로 전달하면 두 메소드 모두 예외가 발생하지 않는다.
            - Parent 객체를 매개값으로 전달하면 `method2()`에서는 `ClassCastException`이 발생
        - `method1()`은 `instanceof` 연산자로 변환이 가능한지 확인한 후 변환을 진행
            - `method2()`의 경우 무조건 변환을 하려 했기 때문에 `ClassCastException`이 발생
            - 예외가 발생하면 프로그램은 즉시 종료되기 때문에 `method1()`과 같이 강제 타입 변환을 하기 전에 `instanceof` 연산자로 변환시킬 타입의 객체인지 조사해서 잘못된 매개값으로 인해 프로그램이 종료되는 것을 막아야 한다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판