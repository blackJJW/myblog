---
title: "[Java] 인터페이스 - 강제 타입 변환(Casting)"
description: ""
date: "2022-08-15T11:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 구현 객체가 인터페이스 타입으로 자동 변환하면, 인터페이스에 선언된 메소드만 사용 가능하다는 제약 사항이 따른다.
    - ex) 인터페이스에는 세 개의 메소드가 선언
        - 인터페이스로 호출 가능한 메소드는 세 개뿐
        
        ![Untitled](/images/lang_java/interface/인터페이스_-_강제_타입_변환(Casting)/Untitled.png)
        
- 경우에 따라서는 구현 클래스에 선언된 필드와 메소드를 사용해야 할 경우도 발생
    - 이때 강제 타입 변환을 해서 다시 구현 클래스 타입으로 변환한 다음, 구현 클래스의 필드와 메소드를 사용 가능
    
    ![Untitled](/images/lang_java/interface/인터페이스_-_강제_타입_변환(Casting)/Untitled%201.png)
    
    ![Untitled](/images/lang_java/interface/인터페이스_-_강제_타입_변환(Casting)/Untitled%202.png)
    
- ex)
    - Vehicle.java
        - 인터페이스
        
        ```java
        public interface Vehicle {
        
        	public void run();
        }
        ```
        
    - Bus.java
        - 구현 클래스
        
        ```java
        public class Bus implements Vehicle{
        
        	@Override
        	public void run() {
        		System.out.println("버스가 달림");
        	}
        	
        	public void checkFare() {
        		System.out.println("승차 요금 체크");
        	}
        }
        ```
        
    - VehicleEx.java
        - 강제 타입 변환
        
        ```java
        public class VehicleEx {
        
        	public static void main(String[] args) {
        		Vehicle vehicle = new Bus();
        		
        		vehicle.run();
        		//vehicle.checkFare(); // (x) 
        		               // Vehicle 인터페이스에는 checkFare()가 없음
        		
        		Bus bus = (Bus) vehicle; // 강제 타입 변환
        		
        		bus.run();
        		bus.checkFare(); // Bus 클래스에는 checkFare()가 있음
        			
        	}
        }
        ```
        
        ![Untitled](/images/lang_java/interface/인터페이스_-_강제_타입_변환(Casting)/Untitled%203.png)
        
        ![Untitled](/images/lang_java/interface/인터페이스_-_강제_타입_변환(Casting)/Untitled%204.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판