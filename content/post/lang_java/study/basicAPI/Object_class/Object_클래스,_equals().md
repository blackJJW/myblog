---
title: "[Java] Object 클래스, equals()"
description: ""
date: "2022-09-17T20:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 클래스를 선언할 때 extends 키워드로 다른 클래스를 상속하지 않으면 암시적으로 java.lang.Object 클래스를 상속하게 된다.
    - 자바의 모든 클래스는 Object 클래스의 자식이거나 자손 클래스이다.
    - Object는 자바의 최상위 부모 클래스에 해당
    
    ![Untitled](/images/lang_java/basicAPI/Object_클래스_equals()/Untitled.png)
    
- Object 클래스는 필드가 없고, 메소드들로 구성
    - 이 메소드들은 모든 클래스가 Object를 상속하기 때문에 모든 클래스에서 사용 가능

---

# 객체 비교(`equals()`)

---

- Object의 `equals()` 메소드
    
    ```java
    public boolean equals(Object obj) { ... }
    ```
    
    - `equals()` 메소드의 매개 타입은 Object
        - 모든 객체가 매개값으로 대입될 수 있음을 의미
        - Object가 최상위 타입이이므로 모든 객체는 Object 타입으로 자동 변환될 수 있기 때문
    - Object 클래스의 `equals()` 메소드는 비교 연산자인 `==`과 동일한 결과를 리턴
        - 두 객체가 동일한 객체라면 true를 리턴, 그렇지 않으면 false를 리턴
        
        ```java
        Object obj1 = new Object();
        Object obj2 = new Objecr();
        
        boolean result = obj1.equals(obj2);
                       //기준       //비교             
        
        boolean result = (obj1 == obj2)
        
        // 결과가 동일
        ```
        
- 자바에서는 두 객체를 동등 비교할 때 `equals()` 메소드를 흔히 사용
    - 두 객체를 비교해서 논리적으로 동등하면 true를 리턴, 그렇지 않으면 false를 리턴
        - 논리적으로 동등하다는 것은 같은 객체이건 다른 객체이건 상관없이 객체가 저장하고 있는 데이터가 동일함을 의미
    - ex) String 객체의 `equals()` 메소드는 String 객체의 번지를 비교하지는 것이 아니고, 문자열이 동일한지 조사하여 같으면 true, 다르면 false를 리턴
        - String 클래스가 Object의 equals() 메소드를 오버라이딩해서 번지 비교가 아닌 문자열 비교로 변경했기 때문
    - Object의 `equals()` 메소드는 직접 사용되지 않고 하위 클래스에서 재정의하여 논리적으로 동등 비교할 때 이용된다.
        - ex) Member 객체는 다르지만 id 필드값이 같으면 논리적으로 동등한 객체로 취급하고 싶은 경우 Object의 `equals()` 메소드를 재정의해서 id 필드값이 같음을 비교
- `equals()` 메소드를 재정의할 때에는 매개값(비교 객체)이 기준 객체와 동일한 타입의 객체인지 먼저 확인해야 한다.
    - Object 타입의 매개 변수는 모든 객체가 매개값으로 제공될 수 있기 때문에 `instanceof` 연산자로 기준 객체와 동일한 타입인지 먼저 확인해야 한다.
    - 만약 비교 객체가 다른 타입일 경우, `equals()`는 false를 리턴해야 한다.
    - 비교 객체가 동일한 타입이라면 기준 객체 타입으로 강제 타입 변환해서 필드값이 동일한지 검사를 한다.
    - 필드값이 동일하다면 true, 다르면 false를 리턴하도록 작성
- ex) 객체 동등 비교(`equals()` 메소드)
    - Member 클래스에서 `equals()` 메소드를 재정의
    - Member 타입이면서 id 필드값이 같을 경우는 true를 리턴하고, 그 이외의 경우에는 false를 리턴
    - Member.java
    
    ```java
    public class Member {
    
    	public String id;
    	
    	public Member(String id) {
    		this.id = id;
    	}
    	
    	@Override
    	public boolean equals(Object obj) {
    		if(obj instanceof Member) { // 매개값이 Member 타입인지 확인
    			Member member = (Member) obj; // Member 타입으로 강제 변환
    			if(id.equals(member.id)) { // id 필드 값이 동일한지 검사한 후, 
    				return true;   // 동일하다면 true를 리턴
    			}
    			
    		}
    		return false; // 매개값이 Member 타입이 아니거나 
    	}                 // id 필드값이 다른 경우 false를 리턴
    }
    ```
    
    - MemberEx.java
    
    ```java
    public class MemberEx {
    
    	public static void main(String[] args) {
    		Member obj1 = new Member("blue");
    		Member obj2 = new Member("blue");
    		Member obj3 = new Member("red");
    		
    		if(obj1.equals(obj2)) { // 매개값이 Member 타입이고 id 필드값도 동일하므로 true
    			System.out.println("obj1과 obj2는 동등");
    		} else {
    			System.out.println("obj1과 obj2는 다름");
    		}
    		
    		if(obj1.equals(obj3)) { // 매개값이 Member 타입이지만 id 필드값이 다르므로 false
    			System.out.println("obj1과 obj3은 동일");
    		} else {
    			System.out.println("obj1과 obj3은 다름");
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/Object_클래스_equals()/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판