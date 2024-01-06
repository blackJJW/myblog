---
title: "[Java] final 클래스와 final 메소드"
description: ""
date: "2022-08-02T19:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- final 키워드는 클래스, , 필드, 메소드 선언 시에 사용 가능
- final 키워드는 해당 선언이 최종 상태이고, 결코 수정될 수 없음을 의미
- 클래스, 필드. 메소드 선언에 사용될 경우 해석이 조금씩 달라짐
    - 필드 선언 시
        - final이 지정되면 초기값 설정 후, 더 이상 값을 변경할 수 없다는 것을 의미
    - 클래스와 메소드 선언 시
        - final 키워드가 지정되면 상속과 관련

## 상속할 수 없는 final 클래스

---

- 클래스를 선언할 때 final 키워드를 class 앞에 붙이게 되면 이 클래스는 최종적인 클래스이므로 상속할 수 없는 클래스가 된다.
    - final 클래스는 부모 클래스가 될 수 없어 자식 클래스를 만들 수 없다.
    
    ```java
    public final class 클래스 { ... }
    ```
    
- final 클래스의 대표적인 예
    - 자바 표준 API에서 제공하는 String 클래스
        
        ```java
        public final class String { ... }
        ```
        
    - 다음과 같이 자식 클래스 생성 불가
        
        ```java
        public class NewString extends String { ... }
        											// 자식 클래스 생성 불가
        ```
        
- ex) Member 클래스 선언 시 final을 지정함으로써 Member를 상속해서 VeryVeryImportantPerson을 선언할 수 없음을 보여준다.
    - Member.java
        - 상속할 수 없는 final 클래스
        
        ```java
        public final class Member {
        	
        }
        ```
        
    - VeryVeryImportantPerson.java
        - 상속할 수 없는 final 클래스
        
        ```java
        // Member를 상속할 수 없음
        public class VeryVeryImportantPerson extends Member{
        
        }
        ```
        
        ![Untitled](/images/lang_java/inheritance/final_클래스와_final_메소드/Untitled.png)
        

## 오버라이딩할 수 없는 final 메소드

---

- 메소드를 선언할 때 final 키워드를 붙이게 되면 이 메소드는 최종적인 메소드이므로 오버라이딩(Overriding)할 수 없는 메소드가 된다.
    - 부모 클래스를 상속해서 자식 클래스를 선언할 때 부모 클래스에 선언된 final 메소드는 자식 클래스에서 재정의할 수 없다는 의미
    
    ```java
    public final 리턴타입 메소드( [매개변수, ... ] ) { ... }
    ```
    
- ex) Car 클래스의 `stop()` 메소드를 final로 선언해서 Car를 상속한 SportsCar 클래스에서 `stop()`메소드를 오버라이딩할 수 없음을 보여준다.
    - Car.java
        - 재정의할 수 없는 final 메소드
        
        ```java
        public class Car {
        	// 필드
        	public int speed;
        	
        	// 메소드
        	public void speedUp() { speed += 1; }
        	
        	// final 메소드
        	public final void stop() {
        		System.out.println("차를 멈춤");
        		speed = 0;
        	}
        }
        ```
        
    - SportsCar.java
        - 재정의할 수 없는 final 메소드
        
        ```java
        public class SportsCar extends Car{
        	@Override
        	public void speedUp() { speed += 10; }
        	
        	// 오버라이딩 할 수 없음
        	@Override
        	public void stop() {
        		System.out.println("스포츠카 멈춤");
        		speed = 0;
        	}
        }
        ```
        
        ![Untitled](/images/lang_java/inheritance/final_클래스와_final_메소드/Untitled%201.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판