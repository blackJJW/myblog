---
title: "[Java] Object 클래스, 객체 복사(clone())"
description: ""
date: "2022-09-19T21:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 원본 객체의 필드값과 동일한 값을 가지는 새로운 객체를 생성하는 것
    - 이유는 원본 객체를 안전하게 보호하기 위해
    - 신뢰하지 않은 영역으로 원본 객체를 넘겨 작업할 경우 원본 객체의 데이터가 훼손될 가능성 있음
        - 복제된 객체를 만들어 신뢰하지 않은 영역으로 넘기는 것이 좋다.
- 복제된 객체의 데이터가 훼손되더라도 원본 객체는 아무런 영향을 받지 않아, 안전하게 데이터를 보호하는 것이 가능
- 객체를 복제하는 방법
    - 얕은 복제
    - 깊은 복제

---

## 얕은 복제(thin clone)

- 단순히 필드값을 복사해서 객체를 복제하는 것
- 필드값만 복제하기 때문에 필드가 기본 타입일 경우 값 복사가 발생
- 필드가 참조 타입일 경우, 객체의 번지가 복사
- ex) 원본 객체에 int 타입의 필드와 배열 타입의 필드가 있을 경우
    
    ![Untitled](/images/lang_java/basicAPI/객체_복사(clone())/Untitled.png)
    
- Object의 clone() 메소드는 자신과 동일한 필드값을 가진 얕은 복재된 객체를 리턴
    - 이 메소드로 객체를 복제하려면 원본 객체는 반드시 `java.lang.Cloneable` 인터페이스를 구현하고 있어야 한다.
    
    ### Cloneable 인터페이스를 명시적으로 구현하는 이유
    
    ---
    
    - 클래스 설계자가 복제를 허용한다는 의도적인 표시를 하기 위해
    - 클래스 설계자가 복제를 허용하지 않는 다면 Cloneable 인터페이스를 구현하지 않으면 된다.
        - 구현하지 않을 시, `clone()` 메소드를 호출할 때 `CloneNotSupportedException` 예외가 발생하여 복제가 실패된다.
    
    ---
    
- `clone()`은 `CloneNotSupportedException` 예외 처리가 필요한 메소드이기 때문에 `try`-`catch` 구문이 필요
    
    ```java
    try {
    	Object obj = clone();
    } catch(CloneNotSupportedException e) {}
    ```
    
- ex) 복제할 수 있는 클래스 선언
    - Member 클래스가 Cloneable 인터페이스를 구현했기 때문에 `getMember()` 메소드에서 `clone()` 메소드로 자신을 복제한 후, 복제한 객체를 외부로 리턴 가능
    - Member1.java
    
    ```java
    public class Member1 implements Cloneable{ 
                                 // 복제 가능 표시
    	public String id;
    	public String name;
    	public String password;
    	public int age;
    	public boolean adult;
    	
    	public Member1(String id, String name, String password, int age, boolean adult) {
    		this.id = id;
    		this.name = name;
    		this.password = password;
    		this.age = age;
    		this.adult = adult;
    	}
    	
    	public Member1 getMember1() {
    		Member1 cloned = null;
    		
    		try {
    			cloned = (Member1) clone(); // clone() 메소드릐 리턴 타입은 
    			                            // Object이므로 Member1 타입으로 캐스팅
    		} catch (CloneNotSupportedException e) {}
    		
    		return cloned;
    		
    	}
    }
    ```
    
    - Member1Ex.java
        - 원본 Member1를 복제한 후, 복제 Member1의 password 필드값을 변경하더라도 Member1의 password 필드값은 변경되지 않음을 보여준다.
        - 복제 객체를 변경하더라도 원본 객체는 변함 없음
    
    ```java
    public class Member1Ex {
    
    	public static void main(String[] args) {
    		// 원본 객체 생성
    		Member1 original = new Member1("blue", "ABC", "12345", 25, true);
    		
    		// 복제 객체를 얻은 후에 패스워드 변경
    		Member1 cloned = original.getMember1();
    		cloned.password = "67890"; // 복제 객체에서 password 변경
    		
    		System.out.println("[ 복제 객체의 필드값 ]");
    		System.out.println("id : " + cloned.id);
    		System.out.println("name : " + cloned.name);
    		System.out.println("password : " + cloned.password);
    		System.out.println("age : " + cloned.age);
    		System.out.println("adult : " + cloned.adult);
    		
    		System.out.println();
    		
    		System.out.println("[ 원본 객체의 필드값 ]");
    		System.out.println("id : " + original.id);
    		System.out.println("name : " + original.name);
    		System.out.println("password : " + original.password);
    		System.out.println("age : " + original.age);
    		System.out.println("adult : " + original.adult);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_복사(clone())/Untitled%201.png)
    

---

## 깊은 복제(deep clone)

- 얕은 복제(thin clone)의 경우 참조 타입 필드는 번지만 복제되기 때문에 원본 객체의 필드와 복제 객체의 필드는 같은 객체를 참조하게 된다.
    - 만약 복제 객체에서 참조 객체를 변경하면 원본 객체도 변경된 객체를 가지게 된다.
    - 얕은 복제의 단점
- 얕은 복제의 반대 개념은 깊은 복제(deep clone)
    - 깊은 복제란 참조하고 있는 객체도 복제하는 것
- ex) 원본 객체를 깊은 복제를 했을 경우 참조하는 배열 객체도 복제된다.
    
    ![Untitled](/images/lang_java/basicAPI/객체_복사(clone())/Untitled%202.png)
    
- 깊은 복제를 하려면 Object의 `clone()` 메소드를 재정의해서 참조 객체를 복제하는 코드를 작성해야 한다.
- ex) `clone()`을 재정의해서 깊은 복제로 변경
    - Member2 클래스에 `int[]` 배열과 Car 타입의 필드가 존재
        - 이 필드들은 모두 참조 타입이므로 깊은 복제 대상
    - Member2 클래스는 Object의 `clone()` 메소드를 재정의해서 `int[]` 배열과 Car 객체를 복제
    - Member2.java
    
    ```java
    import java.util.Arrays;
    
    public class Member2 implements Cloneable{
    
    	public String name;
    	public int age;
    	public int[] scores; // 참조 타입 필드
    	public Car car;      // (깊은 복제 대상)
    	
    	public Member2(String name, int age, int[] scores, Car car) {
    		this.name = name;
    		this.age = age;
    		this.scores = scores;
    		this.car = car;
    	}
    	
    	// clone() 메소드 재정의
    	@Override
    	protected Object clone() throws CloneNotSupportedException {
    		// 먼저 얕은 복사를 해서 name, age를 복제
    		Member2 cloned = (Member2) super.clone(); // Object의 clone() 호출
    		// scores를 깊은 복제한다.
    		cloned.scores = Arrays.copyOf(this.scores, this.scores.length);
    		// car를 깊은 복제한다.
    		cloned.car = new Car(this.car.model);
    		// 깊은 복제된 Member 객체를 리턴
    		return cloned;
    	}
    	
    	public Member2 getMember2() {
    		Member2 cloned = null;
    		try {
    			// 재정의된 clone() 메소드 호출
    			cloned = (Member2) clone();
    		} catch(CloneNotSupportedException e) {
    			e.printStackTrace();
    		}
    		return cloned;
    	}
    }
    ```
    
    - Car.java
        - Car 클래스
    
    ```java
    public class Car {
    	public String model;
    	
    	public Car(String model) {
    		this.model = model;
    	}
    
    }
    ```
    
- ex) 깊은 복제 후 복제본 변경은 원본에 영향을 미치지 않는다.
    - Member2 클래스의 `getMember2()` 메소드를 호출해서 복제된 Member2 객체를 얻은 후에 scores 배열 항목값과 car 객체의 모델을 변경
    - 원본 Member2 객체의 scores 배열 항목값과 car 객체의 모델은 변함이 없는 것을 확인
        - 원본과 복제본이 각각 참조하는 scores 배열과 car 객체는 서로 다르기 때문
    - Member2Ex.java
    
    ```java
    public class Member2Ex {
    
    	public static void main(String[] args) {
    		// 원본 객체 생성
    		Member2 original = new Member2("ABC", 23, new int[] {90, 90}, new Car("그랜져"));
    		
    		// 깊은 복제 후 참조 객체의 데이터를 변경
    		// 복제 객체를 얻은 후에 참조 객체의 데이터를 변경
    		Member2 cloned = original.getMember2();
    		cloned.scores[0] = 100;
    		cloned.car.model = "소나타";
    		
    		System.out.println("[ 복제 객체의 필드값 ]");
    		System.out.println("name : " + cloned.name);
    		System.out.println("age : " + cloned.age);
    		System.out.print("scores : {");
    		for(int i = 0; i < cloned.scores.length; i++) {
    			System.out.print(cloned.scores[i]);
    			System.out.print((i == (cloned.scores.length - 1)) ? "" : ",");
    		}
    		System.out.println("}");
    		System.out.println("car : " + cloned.car.model);
    		
    		System.out.println();
    		
    		System.out.println("[ 원본 객체의 필드값 ]");
    		System.out.println("name : " + original.name);
    		System.out.println("age : " + original.age);
    		System.out.print("scores : {");
    		for(int i = 0; i < original.scores.length; i++) {
    			System.out.print(original.scores[i]);
    			System.out.print((i==(original.scores.length - 1)) ? "" : ",");
    		}
    		System.out.println("}");
    		System.out.println("car : " + original.car.model);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_복사(clone())/Untitled%203.png)
    
---
    
## References
    
- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판