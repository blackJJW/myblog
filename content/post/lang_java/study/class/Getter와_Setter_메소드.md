---
title: "[Java] Getter와 Setter 메소드"
description: ""
date: "2022-07-28T17:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 일반적으로 객체 지향 프로그래밍에서 객체의 제이터는 객체 외부에서 직접적으로 접근하는 것을 막는다.
    - 객체의 데이터를 외부에서 마음대로 읽고 변경할 경우 객체의 무결성이 깨어질 수 있음
    - ex) 자동차의 속도는 음수가 될 수 없다
        - 외부에서 음수로 변경하면 객체의 무결성이 깨진다.
        
        ```java
        myCar.speed = -100;
        ```
        

### Setter

- 이러한 문제를 해결하기 위해 객체 지향 프로그래밍에서는 메소드를 통해 데이터를 변경하는 방법을 선호
    - 데이터는 외부에서 접근할 수 없도록 막고 메소드는 공개해서 외부에서 메소드를 통해 데이터테 접근하도록 유도
    - 메소드는 매개값을 검증해서 유효한 값만 데이터로 저장할 수 있기 때문
    - 이러한 역할을 하는 메소드가 `Setter`
    - ex) 자동차의 속도를 `setSpeed()` 메소드로 변경할 경우 다음과 같이 검증 코드 작성
        
        ```java
        void setSpeed(double speed) {
        	if(speed < 0) { // 매개값이 음수일 경우, speed 필드에 0으로 저장, 메소드 종료
        		this.speed = 0;
        		return;
        	} else {
        		this.speed = speed;
        	}
        }
        ```
        

---

### Getter

- 외부에서 객체의 데이터를 읽을 때도 메소드를 사용하는 것이 좋다.
- 객체 외부에서 객체의 필드값을 사용하기에 부적절한 경우도 존재
    - 이런 경우 메소드로 필드값을 가공한 후 외부로 전달하면 된다.
    - 이런 메소드가 `Getter`
    - ex) 자동차의 속도를 마일에서 KM 단위로 환산해서 외부로 리턴해주는 `getSpeed()` 메소드를 다음과 같이 작성
        
        ```java
        double getSpeed() { // 필드값인 마일을 km 단위로 환산 후 외부로 리턴
        	double km = speed * 1.6;
        	return km;
        }
        ```
        

---

- 클래스를 선언할 때 가능하다면 필드를 `private`로 선언해서 외부로부터 보호하고, 필드에 대한 `Setter`와 `Getter` 메소드를 작성해서 필드값을 안전하게 변경 / 사용하는 것이 좋다.

### Setter와 Getter 메소드를 선언하는 방법

- 검증 코드나 변환 코드는 필요에 따라 추가

```java
private 타입 fieldName;  // 접근 제한자 : private

// Getter
public 리턴 타입 getFieldName() { // 접근 제한자 : public
	return fieldName;              // 리턴 타입 : 필드 타입
}                                // 메소드 이름 : get + 필드이름(첫문자 대문자)
                                 // 리턴값 : 필드값

// Setter
public void setFieldName(타입 fieldName) { // 접근 제한자 : public
	this.fieldName = fieldName;              // 리턴 타입 : void
}                                          // 메소드 이름 : set + 필드이름(첫문자 대문자)
                                           // 매개 변수 타입 : 필드타입
```

- 필드 타입이 boolean일 경우에는 `Getter`는 get으로 시작하지 않고 is로 시작하는 것이 관례
    - ex) stop 필드의 `Getter`와 `Setter`
        
        ```java
        private boolean stop; // 필드 접근 제한자 : private
        
        // Getter
        public boolean isStop() { // 접근 제한자 : public
        	return stop;            // 리턴 타입 : 필드타입
        }                         // 메소드 이름 : is + 필드이름(첫문자 대문자)
                                  // 리턴값 : 필드값
        
        // Setter
        public int setStop(boolean stop) { // 접근 제한자 : public
        	this.stop = stop;                // 리턴 타입 : void
        }                                  // 메소드 이름 : set + 필드이름(첫문자 대문자)
                                           // 매개 변수 타입 : 필드타입
        ```
        
- 외부에서 필드값을 읽을 수만 있고 변경하지 못하도록 하려면(읽기 전용)
    - `Getter` 메소드만 선언
    - `Setter` 메소드를 `private` 접근 제한을 갖도록 선언

### 이클립스에서 자동적으로 Getter와 Setter 메소드를 생성

- 이클립스는 클래스에 선언된 필드에 대해 자동적으로 `Getter`와 `Setter` 메소드를 생성시키는 기능이 존재
- 필드를 선언한 후 메뉴에서
    
    [ Source → Generate Getters and Setters ]
    
    ![Untitled](/images/lang_java/class/Getter와_Setter_메소드/Untitled.png)
    
    - 선언된 필드에 대한 `Getter`와 `Setter`를 자동으로 생성시킬수 있는 대화상자가 실행
        
        ![Untitled](/images/lang_java/class/Getter와_Setter_메소드/Untitled%201.png)
        
        - 원하는 필드를 선택 후 Generate 를 클릭한다.

---

- ex)  Getter와 Setter 메소드 선언
    - Car.java
        
        ```java
        package getterAndSetter;
        
        public class Car {
        	// 필드
        	private int speed;
        	private boolean stop;
        	
        	// 생성자
        	
        	// 메소드
        	
        	public int getSpeed() {
        		return speed;
        	}
        	public void setSpeed(int speed) {
        		if(speed < 0) {
        			this.speed = 0;
        			return;
        		} else {
        			this.speed = speed;
        		}
        	}
        	public boolean isStop() {
        		return stop;
        	}
        	public void setStop(boolean stop) {
        		this.stop = stop;
        		this.speed = 0;
        	}
        }
        ```
        
    - CarEx.java
        
        ```java
        package getterAndSetter;
        
        public class CarEx {
        
        	public static void main(String[] args) {
        		Car myCar = new Car();
        		
        		// 잘몫된 속도 변경
        		myCar.setSpeed(-50);
        		
        		System.out.println("현재 속도 : " + myCar.getSpeed());
        		
        		// 올바른 속도 변경
        		myCar.setSpeed(60);
        		
        		// 멈춤
        		if(!myCar.isStop()) {
        			myCar.setStop(true);
        		}
        		System.out.println("현재 속도 : " + myCar.getSpeed());
        
        	}
        }
        ```
        
        ![Untitled](/images/lang_java/class/Getter와_Setter_메소드/Untitled%202.png)
        
        - CarEx 클래스에서 비정상적인 속도값으로 변경 시도
            - speed 필드의 `Setter(setSpeed())`에서 매개값을 검사한 후 0으로 변경
            - `Getter(getSpeed())`의 리턴값은 0
        - stop 필드의 `Getter(isStop())` 리턴값이 false일 경우
            - 자동차를 멈추기 위해 `Setter(setStop(true))`를 호출해서 stop 필드를 true로, speed 필드를 0으로 변경

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판