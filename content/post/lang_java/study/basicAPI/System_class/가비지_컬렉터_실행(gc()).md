---
title: "[Java] System 클래스, 가비지 컬렉터 실행(gc())"
description: ""
date: "2022-09-21T21:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 자바는 개발자가 메모리를 직접 코드로 관리하지 않고 JVM이 자동으로 관리한다.
- JVM은 메모리가 부족할 때와 CPU가 한가할 때에 가비지 컬렉터(Garbage Collector)를 실행
    - 사용하지 않는 객체를 자동으로 제거
- ex)
    - `new` 연산자로 Car 객체를 생성하고 변수 myCar에 객체 번지를 대입했다고 가정
        
        ```java
        Car myCar = new Car();
        ```
        
        ![Untitled](/images/lang_java/basicAPI/가비지_컬렉터_실행(gc())/Untitled.png)
        
    - 만약 변수 myCar에 null을 대입하면, myCar는 객체의 번지를 잃게 된다.
        - 객체의 번지를 모르니, 더 이상 Car 객체는 사용할 수 없고, 쓰레기가 된다.
        
        ```java
        myCar = null;
        ```
        
        ![Untitled](/images/lang_java/basicAPI/가비지_컬렉터_실행(gc())/Untitled%201.png)
        
    - 변수 myCar가 다른 Car 객체를 참조할 경우도 마찬가지
        - 이전 객체의 번지를 잃기 때문에 이전 객체는 쓰레기가 된다.
        
        ```java
        Car myCar = new Car(): // 이전 참조 객체
        myCar = new Car();     // 현재 참조 객체
        ```
        
        ![Untitled](/images/lang_java/basicAPI/가비지_컬렉터_실행(gc())/Untitled%202.png)
        
- 가비지 컬렉터는 개발자가 직접 코드로 실행시킬 수 없다.
    - 대신 JVM에게 가능한한 빨리 실행해 달라고 요청할 수는 있다.
    - 이것이 `System.gc()` 메소드
    - `System.gc()` 메소드가 호출되면 쓰레기 수집기가 바로 실행되는 것은 아니고, JVM은 빠른 시간 내에 실행시키기 위해 노력한다.
    
    ```java
    System.gc();
    ```
    
- 쓰레기가 생길 때마다 쓰레기 수집기가 동작한다면 수행되어야 할 프로그램 속도가 떨어지기 때문에 성능 측명에서 좋지 않다.
    - 메모리가 충분하다면 굳이 가비지 컬렉터를 실행할 필요가 없다.
    - `gc()` 메소드는 메모리가 열악하지 않은 환경이라면 거의 사용할 일이 없다.
- ex) gc 메소드
    - 가비지 컬렉터가 객체를 삭제하는지 확인
    - 객체 소멸자(`finalize()`)에서 학습한 소멸자를 이용
    - 가비지 컬렉터는 객체를 삭제하기 전에 마지막으로 객체의 소멸자를 실행
    - GcEx.java
    
    ```java
    public class GcEx {
    
    	public static void main(String[] args) {
    		Employee emp;
    		
    		emp = new Employee(1); // 쓰레기가 됨
    		emp = null;
    		emp = new Employee(2); // 쓰레기가 됨
    		emp = new Employee(3);
    		
    		System.out.print("emp가 최종적으로 참조하는 사원번호 : ");
    		System.out.println(emp.eno);
    		System.gc(); // 가비지 컬렉터 실행 요청
    
    	}
    
    }
    class Employee{
    	public int eno;
    	
    	public Employee(int eno) { 
    		this.eno = eno;
    		System.out.println("Employee(" + eno + ") 이 메모리에서 생성됨");
    	}
    	
    	public void finalize() { // 소멸자
    		System.out.println("Employee(" + eno + ") 이 메모리에서 제거됨");
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/가비지_컬렉터_실행(gc())/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판