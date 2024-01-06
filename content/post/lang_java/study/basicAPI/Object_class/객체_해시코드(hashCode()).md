---
title: "[Java] Object 클래스, 객체 해시코드(hashCode())"
description: ""
date: "2022-09-18T15:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 객체 해시코드 : 객체를 식별할 하나의 정수값을 말함.
- Object의 hashCode() 메소드는 객체의 메모리 번지를 이용하여, 해시코드를 만들어 리턴하기 때문에 객체마다 다른 값을 가지고 있다.
- 논리적 동등 비교 시 `hashCode()`를 오버라이딩할 필요가 있음
    - 컬렉션 프레임워크에서 HashSet, HashMap, Hashtable은 다음과 같은 방법으로 두 객체가 동등한지 비교한다.
        1. `hashCode()` 메소드를 실행해서 리턴된 해시코드 값이 같은지를 확인
        2. 해시코드 값이 다르면 다른 객체로 판단, 
            1. 해시코드 값이 같을 경우 `equals()` 메소드로 다시 비교
            2. hashCode9) 메소드가 true가 나와도 equals()의 리턴값이 다르면 다른 객체가 된다.
        
        ![Untitled](/images/lang_java/basicAPI/객체_해시코드(hashCode())/Untitled.png)
        
- ex) `hashCode()` 메소드를 재정의하지 않음
    - Key 클래스는 `equals()` 메소드를 재정의해서 number 필드값이 같으면 true를 리턴
    - `hashCode()` 메소드는 재정의하지 않았기 때문에 Object의 `hashCode()` 메소드가 사용
    - key.java
    
    ```java
    public class Key {
    	public int number;
    	
    	public Key(int number) {
    		this.number = number;
    	}
    	
    	@Override
    	public boolean equals(Object obj) {
    		if(obj instanceof Key) {
    			Key compareKey = (Key) obj;
    			if(this.number == compareKey.number) {
    				return true;
    			}
    		}
    		return false;
    	}
    
    }
    ```
    
    - 이런 경우 HashMap의 식별키로 Key 객체를 사용하면 저장된 값을 찾아오지 못한다.
        - number 필드값이 같더라도 `hashCode()` 메소드에서 리턴하는 해시코드가 다르기 때문에 다른 식별키로 인식하기 때문
        
- ex) 다른 키로 인식
    - “`new Key(1)`” 객체로 “`ABC`”를 저장하고, 다시 “`new Key(1)`”객체로 저장된 “`ABC`”를 읽을려고 하면 null이 나온다.
    - KeyEx.java
    
    ```java
    import java.util.HashMap;
    
    public class KeyEx {
    
    	public static void main(String[] args) {
    		// Key 객체를 식별키로 사용해서 String 값을 저장하는 HashMap 객체 생성
    		HashMap<Key, String> hashMap = new HashMap<Key, String>();
    		
    		// 식별키 "new Key(1)"로 "ABC"를 저장
    		hashMap.put(new Key(1), "ABC");
    		
    		// 식별키 "new Key(1)"로 "ABC"을 읽어옴
    		String value = hashMap.get(new Key(1));
    		System.out.println(value);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_해시코드(hashCode())/Untitled%201.png)
    

- ex) `hashCode()` 메소드 재정의 추가
    - 의도한 대로 “ABC”를 읽으려면 재정의한 `hashCode()` 메소드를 Key 클래스에 추가하면 된다.
    - `hashCode()`의 리턴값을 number 필드값으로 했기 때문에 저장할 때의 “`new Key(1)`”과 읽을 때의 “`new Key(1)`”은 같은 해시코드가 리턴
    - Key.java
    
    ```java
    public class Key {
    	public int number;
    	
    	public Key(int number) {
    		this.number = number;
    	}
    	
    	@Override
    	public boolean equals(Object obj) {
    		if(obj instanceof Key) {
    			Key compareKey = (Key) obj;
    			if(this.number == compareKey.number) {
    				return true;
    			}
    		}
    		return false;
    	}
    	
    	@Override
    	public int hashCode() {
    		return number;
    	}
    
    }
    ```
    
    - 저장할 때의 `new Key(1)`과 읽을 때의 `new Key(1)`은 사실 서로 다른 객체이지만 HashMap은 `hashCode()`의 리턴값이 같다
        - `equals()` 리턴값이 true가 나오기 때문에 동등 객체로 평가
            - 같은 식별키로 인식한다는 의미
    - 객체의 동등 비교를 위해서는 Object의 `equals()`만 재정의하지 말고 `hashCode()` 메소드도 재정의해서 논리적 동등 객체일 경우 동일한 해시코드가 리턴되도록 해야 한다.
- ex) `hashCode()` 메소드 재정의 추가
    - id 필드값이 같을 경우 같은 해시코드를 리턴하도록 하기 위해 String의 `hashCode()` 메소드를의 리턴값을 활용
    - String의 `hashCode()`는 같은 문자열일 경우 동일한 해시코드를 리턴
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
    	//---------------------------------------
    	@Override
    	public int hashCode() { // id가 동일한 문자열인 경우 같은 해시 코드를 리턴
    		return id.hashCode();
    	}
      //-----------------------------------------
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판