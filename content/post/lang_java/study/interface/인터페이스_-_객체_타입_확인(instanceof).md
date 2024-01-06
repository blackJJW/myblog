---
title: "[Java] 인터페이스 - 객체 타입 확인(instanceof)"
description: ""
date: "2022-08-15T12:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 강제 타입 변환은 구현 객체가 인터페이스 타입으로 변환되어 있는 상태에서 가능
- 구현 객체가 변환되어 있는지 알 수 없는 상태에서 무작정 변환을 할 경우 ClassCastException 발생 가능
    - ex) Taxi 객체가 인터페이스로 변환되어 있을 경우
        - Bus 타입으로 강제 타입 변환하면 구현 클래스 타입이 다르므로 ClassCastException이 발생
        
        ```java
        Vehicle vehicle = new Taxi();
        Bus bus = (Bus) vehicle;
        ```
        
        ![Untitled](/images/lang_java/interface/인터페이스_-_객체_타입_확인(instanceof)/Untitled.png)
        
    
    ---
    
- 메소드의 매개 변수가 인터페이스로 선언된 경우, 메소드를 호출할 때 다양한 구현 객체들을 매개값으로 지정할 수 있다(매개 변수의 다형성).
    - 어떤 구현 객체가 지정될지 모르는 상황에서 매개값을 Bus로 강제 타입 변환하면 ClassCastException이 발생 가능
    
    ```java
    public void drive(Vehicle vehicle){
    	Bus bus = (Bus) vehicle;
    	bus.checkFare();
    	vehicle.run();
    }
    ```
    

---

### 어떤 구현 객체가 인터페이스 타입으로 변환되었는지 확인하는 방법

- 상속에서 객체 타입을 확인하기 위해 `instanceof` 연산자를 사용
- `instanceof` 연산자는 인터페이스 타입에서도 사용 가능
    - ex) Vehicle 인터페이스 타입으로 변환된 객체가 Bus인지 확인
        
        ```java
        if (vehicle instanceof Bus ) {
        	Bus.bus = (Bus) vehicel;
        }
        ```
        
- 인터페이스 타입으로 자동 변환된 매개값을 메소드 내에서 다시 구현 클래스 타입으로 강제 타입 변환해야 할 경우
    - 매개값이 어떤 객체인지 `instanceof` 연산자로 확인하고 안전하게 강제 타입 변환을 해야 한다.
- ex) `drive()` 메소드에서 매개값이 Bus 객체인 경우
    - Bus의 `checkFare()` 메소드를 호출해야 한다면 Bus 타입으로 강제 타입 변환을 해야 한다.
    - Vehicle 인터페이스에는 `checkFare()` 메소드가 없기 때문
    - 매개값으로 어떤 구현 객체가 대입될지 모르기 때문에 `instanceof` 연산자로 Bus 타입인지 확인해야 한다.
    - Driver.java
        - 객체 타입 확인
        
        ```java
        public class Driver {
        
        	public void driver(Vehicle vehicle) {
        		if(vehicle instanceof Bus) { // vehicle 매개 변수가
        			                         // 참조하는 객체가 Bus인지 조사
        			Bus bus = (Bus) vehicle; // Bus 객체일 경우 안전하게 강제 타입 변환
        			bus.checkFare(); // Bus 타입으로 강제 타입 변환을 하는 이유
        		}
        		vehicle.run();
        	}
        }
        ```
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판