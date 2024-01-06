---
title: "[Java] protected 접근 제한자"
description: ""
date: "2022-08-02T20:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 접근 제한자는 `public`, `protected`, `default`, `private`와 같이 네 가지 종류가 있다.
    - `protected`는 상속과 관련
    
    ![Untitled](/images/lang_java/inheritance/protected_접근_제한자/Untitled.png)
    
    ![Untitled](/images/lang_java/inheritance/protected_접근_제한자/Untitled%201.png)
    
- `protected`는 `public`과 `default` 접근 제한의 중간쯤에 해당
    - 같은 패키지에서는 default와 같이 접근 제한이 없지만 다른 패키지에서는 자식 클래스만 접근을 허용
    
    ![Untitled](/images/lang_java/inheritance/protected_접근_제한자/Untitled%202.png)
    
- protected는 필드와 생성자, 메소드 선언에 사용 가능
- ex)
    - A.java
        - `protected`로 선언된 필드, 생성자, 메소드가 있다.
        
        ```java
        package inheritance.package1;
        
        public class A {
        	protected String field;
        	
        	protected A() {
        		
        	}
        	
        	protected void method() {
        		
        	}
        }
        ```
        
    - B.java
        - A 클래스와 동일한 패키지
        - `default` 접근 제한과 마찬가지로 B 클래스의 생성자와 메소드에서는 A 클래스의 `protected` 필드, 생성자, 메소드에 얼마든지 접근 가능
        
        ```java
        package inheritance.package1;
        
        public class B {
        	public void method() {
        		A a = new A();     // (o)
        		a.field = "value"; // (o)
        		a.method();        // (o)
        	}
        }
        ```
        
    - C.java
        - A 클래스와 다른 패키지에 존재
        - `default` 접근 제한과 마찬가지로 C 클래스의 생성자와 메소드에서는 A 클래스의 `protected` 필드, 생성자, 메소드에 얼마든지 접근 불가능
        
        ```java
        package inheritance.package2;
        
        import inheritance.package1.A;
        
        public class C {
        	public void method() {
        		A a = new A();     // (x)
        		a.field = "value"; // (x)
        		a.method();        // (x)
        	}
        }
        ```
        
        ![Untitled](/images/lang_java/inheritance/protected_접근_제한자/Untitled%203.png)
        
    - D.java
        - A 클래스와 다른 패키지에 존재
        - C 클래스와 달리 D는 A의 자식 클래스
            - A 클래스의 `protected` 필드, 생성자, 메소드에 접근 가능
        - 단, new 연산자를 사용해서 생성자를 직접 호출할 수 없고, 자식 생성자를 호출 가능
        
        ```java
        package inheritance.package2;
        
        import inheritance.package1.A;
        
        public class D extends A{
        	public D() {
        		super();              // (o)
        		this.field = "value"; // (o)
        		this.method();        // (o)
        	}
        }
        ```
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판