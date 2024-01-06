---
title: "[Java]  Map 컬렉션, HashMap"
description: ""
date: "2022-11-13T14:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Map 인터페이스를 구현한 대표적인 Map 컬렉션
- HashMap의 키로 사용할 객체는 `hashCode()`와 `equals()` 메소드를 재정의해서 동등 객체가 될 조건을 정해야 한다.
    - 동등 객체, 즉 동일한 키가 될 조건
        - `hashCode()`의 리턴값이 같아야 한다.
        - `equals()` 메소드가 true를 리턴해야 한다.
    
    ![Untitled](/images/lang_java/collectionFramework/Map_컬렉션/HashMap/Untitled.png)
    
- 주로 키 타입은 String을 많이 사용
    - String은 문자열이 같을 경우 동등 객체가 될 수 있도록 `hashCode()`와 `equals()` 메소드가 재정의되어 있다.
- HashMap을 생성하기 위해서는 키 타입과 값 타입을 파라미터로 주고 기본 생성자를 호출
    
    ```java
    Map<K, V> map = new HashMap<K, V>();
    // K : 키 타입
    // V : 값 타입
    ```
    
    - 키와 값의 타입은 기본 타입(byte, short, int, float, double, boolean, char)을 사용 불가
        - 클래스 및 인터페이스 타입만 가능
    - 키로 String 타입을 사용하고 값으로 Integere 타입을 사용하는 HashMap은 다음과 같이 생성 가능
        
        ```java
        Map<String, Integer> map = new HashMap<String, Integer>();
        ```
        
- ex) 이름을 키로, 점수를 값으로 저장
    - 이름을 키로, 점수를 값으로 저장하는 HashMap 사용 방법
    - HashMapEx1.java
    
    ```java
    import java.util.HashMap;
    import java.util.Iterator;
    import java.util.Map;
    import java.util.Set;
    
    public class HashMapEx1 {
    
    	public static void main(String[] args) {
    		// Map 컬렉션 생성
    		Map<String, Integer> map = new HashMap<String, Integer>();
    		
    		// 객체 저장
    		map.put("ABC", 85);
    		map.put("DEF", 90);
    		map.put("GHI", 80);
    		map.put("DEF", 95); /* DEF 키가 같기 때문에 제일 마지막에 저장한
    		                     * 값으로 대치
    		                     */
    		System.out.println("총 Entry 수 : " + map.size()); // 저장된 총 Entry 수 얻기
    		
    		// 객체 찾기
    		System.out.println("\tABC : " + map.get("ABC")); // 키로 값을 검색
    		System.out.println();
    		
    		// 객체를 하나씩 처리
    		Set<String> keySet = map.keySet(); // Key Set 얻기
    		
    		// 반복해서 키를 얻고 값을 Map에서 얻어냄
    		Iterator<String> keyIterator = keySet.iterator();
    		while(keyIterator.hasNext()) {
    			String key = keyIterator.next();
    			Integer value = map.get(key);
    			System.out.println("\t" + key + " : " + value);
    		}
    		System.out.println();
    		
    		// 객체 삭제
    		map.remove("ABC"); // 키로 Map.Entry를 제거
    		System.out.println("총 Entry 수 : " + map.size());
    		
    		// 객체를 하나씩 처리
    		Set<Map.Entry<String, Integer>> entrySet = map.entrySet(); // Map.Entry Set 얻기
    		Iterator<Map.Entry<String, Integer>> entryIterator = entrySet.iterator();
    		
    		// 반복해서 Map.Entry를 얻고 키와 값을 얻어냄
    		while(entryIterator.hasNext()) {
    			Map.Entry<String, Integer> entry = entryIterator.next();
    			String key = entry.getKey();
    			Integer value = entry.getValue();
    			System.out.println("\t" + key + " : " + value);
    		}
    		System.out.println();
    		
    		// 객체 전체 삭제
    		map.clear(); // 모든 Map.Entry 삭제
    		System.out.println("총 Entry 수 : " + map.size());
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/Map_컬렉션/HashMap/Untitled%201.png)
    
- ex) 키로 사용할 객체 - `hashCode()`와 `equals()` 재정의
    - 사용자 정의 객체인 Student를 키로하고 검수를 저장하는 HashMap 사용 방법
    - 학번과 이름이 동일한 Student를 동등 키로 간주하기 위해 Student 클래스에는 `hashCode()`와 `equals()` 메소드가 재정의
    - Student.java
    
    ```java
    public class Student {
    	public int sno;
    	public String name;
    	
    	public Student(int sno, String name) {
    		this.sno = sno;
    		this.name = name;
    	}
    	
    	public boolean equals(Object obj) { // 학번과 이름이 동일한 경우 true를 리턴
    		if(obj instanceof Student) {
    			Student student = (Student) obj;
    			return (sno == student.sno) && (name.equals(student.name));
    		} else {
    			return false;
    		}
    	}
    	
    	public int hashCode() { // 학번과 이름이 같다면 동일한 값을 리턴
    		return sno + name.hashCode();
    	}
    }
    ```
    
- ex) 학번과 이름이 동일한 경우 같은 키로 인식
    - HashMapEx2.java
    
    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class HashMapEx2 {
    
    	public static void main(String[] args) {
    		Map<Student, Integer> map = new HashMap<Student, Integer>();
    		
    		map.put(new Student(1, "ABC"), 95);
    		map.put(new Student(1, "ABC"), 95);
    		// 학번과 이름이 동일한 Student를 키로 저장
    		
    		// 저장된 총 Map.Entry 수 얻기
    		System.out.println("총 Entry 수 : " + map.size());
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/Map_컬렉션/HashMap/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판