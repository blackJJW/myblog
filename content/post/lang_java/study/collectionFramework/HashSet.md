---
title: "[Java]  Set 컬렉션, HashSet"
description: ""
date: "2022-11-12T13:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Set 인터페이스의 구현 클래스
- HashSet을 생성하기 위해서는 다음과 같이 기본 생성자를 호출하면 된다.
    
    ```java
    Set<E> set = new HashSet<E>();
    ```
    
    - 타입 파라미터 E에는 컬렉션에 저장할 객체 타입을 지정하면 된다.
    - ex) String 객체를 저장하는 HashSet은 다음과 같이 생성
    
    ```java
    Set<String> set = new HashSet<String>();
    ```
    
- HashSet은 객체들을 순서 없이 저장하고 동일한 객체는 중복 저장하지 않는다.
    - HashSet이 판단하는 동일한 객체란 꼭 같은 인스턴스를 뜻하지는 않는다.
- HashSet은 객체를 저장하기 전에 먼저 객체의 `hashCode()` 메소드를 호출해서 해시코드를 얻어낸다.
    - 이미 저장되어 있는 객체들의 해시코드와 비교
    - 만약 동일한 해시코드가 있다면 다시 `equals()` 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장을 하지 않는다.
    
    ![Untitled](/images/lang_java/collectionFramework/Set_컬렉션/HashSet/Untitled.png)
    
- 문자열을 HashSet에 저장할 경우
    - 같은 문자열을 갖는 String 객체는 동등한 객체로 간주되고 다른 문자열을 갖는 String 객체는 다른 객체로 간주
    - 그 이유는 String 클래스가 `hashCode()`와 `equals()` 메소드를 재정의
        - 같은 문자열일 경우 `hashCode()`의 리턴값을 같게
        - `equals()`의 리턴값은 true가 나오도록 했기 때문
- ex) String 객체를 중복 없이 저장하는 HashSet
    - HashSet에  String 객체를 추가, 검색, 제거하는 방법
    - HashSetEx1.java
    
    ```java
    import java.util.HashSet;
    import java.util.Iterator;
    import java.util.Set;
    
    public class HashSetEx1 {
    
    	public static void main(String[] args) {
    		Set<String> set = new HashSet<String>();
    		
    		set.add("Java");
    		set.add("JDBC");
    		set.add("Servlet/JSP");
    		set.add("Java"); // Java는 한 번만 저장됨
    		set.add("iBATIS");
    		
    		int size = set.size(); // 저장된 객체 수 증가
    		System.out.println("총 객체 수 : " + size);
    		
    		Iterator<String> iterator = set.iterator(); // 반복자 얻기
    		while(iterator.hasNext()) { // 객체 수만큼 루핑
    			String element = iterator.next(); // 한 개의 객체를 가져온다. 
    			System.out.println("\t" + element);
    		}
    		
    		set.remove("JDBC");   // 한 개의 객체 삭제
    		set.remove("iBATIS"); // 한 개의 객체 삭제
    		
    		System.out.println("총 객체 수 : " + set.size()); // 저장된 객체 수 얻기
    		
    		iterator = set.iterator(); // 반복자 얻기
    		while(iterator.hasNext()) { // 객체 수만큼 루핑
    			String element = iterator.next();
    			System.out.println("\t" + element);
    		}
    		
    		set.clear(); // 모든 객체를 제거하고 비움
    		if(set.isEmpty()) { System.out.println("비어 있음"); }
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/Set_컬렉션/HashSet/Untitled%201.png)
    
- ex) `hashCode()`와 `equals()` 메소드 재정의
    - 사용자 정의 클래스인 Member를 생성
    - `hashCode()`와 `equals()` 메소드를 오버라이딩
    - 인스턴스가 달라도 이름과 나이가 동일하다면 동일한 객체로 간주하여 중복 저장되지 않도록 하기 위해서이다.
    - Member.java
    
    ```java
    public class Member {
    	public String name;
    	public int age;
    	
    	public Member(String name, int age) {
    		this.name = name;
    		this.age = age;
    	}
    	
    	@Override
    	public boolean equals(Object obj) { /* name과 age 값이 같으면 
    	                                     *  true를 리턴
    	                                     */ 
    		if(obj instanceof Member) {
    			Member member = (Member) obj;
    			return member.name.equals(name) && (member.age == age);
    		} else {
    			return false;
    		}
    	}
    	
    	@Override
    	public int hashCode() { /* name과 age 값이 같으면
    	                         * 동일한 hashCode가 리턴
    	                         */
    		return name.hashCode() + age;
    		      // String의 hashCode() 이용
    	}
    }
    ```
    
- ex) Member 객체를 중복없이 저장하는 HashSet
    - HashSetEx2.java
    
    ```java
    import java.util.HashSet;
    import java.util.Set;
    
    public class HashSetEx2 {
    
    	public static void main(String[] args) {
    		Set<Member> set = new HashSet<Member>();
    		
    		// 인스턴스는 다르지만 내부 데이터가 동일하므로 객체 1개만 저장
    		set.add(new Member("ABC", 30));
    		set.add(new Member("ABC", 30));
    		
    		System.out.println("총 객체 수 : " + set.size()); // 저장된 객체 수 얻기
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/Set_컬렉션/HashSet/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판