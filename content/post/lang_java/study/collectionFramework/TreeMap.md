---
title: "[Java] 검색 기능을 강화시킨 컬렉션, TreeMap"
description: ""
date: "2022-11-18T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- TreeMap은 이진 트리를 기반으로 한 Map 컬렉션
- TreeSet과의 차이점은 키와 값이 저장된 `Map.Entry`를 저장한다는 점
- TreeMap에 객체를 저장하면 자동으로 정렬
    - 기본적으로 부모 키값과 비교
        - 키 값이 낮은 것은 왼쪽 자식 노드에 Map.Entry 객체를 저장
        - 키 값이 높은 것은 오른쪽 자식 노드에 `Map.Entry` 객체를 저장
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeMap/Untitled.png)
    
- TreeMap을 생성하기 위해서는 키로 저장할 객체 타입과 값으로 저장할 객체 타입을 타입 파라미터로 주고 기본 생성자를 호출하면 된다.
    
    ```java
    TreeMap<K, V> treeMap = new TreeMap<K, V>();
    // K : 키 타입
    // V : 값 타입
    ```
    
    - 키로 String 타입을 사용하고 값으로 Integer 타입을 사용하는 TreeMap은 다음과 같이 생성 가능
        
    ```java
    TreeMap<String, Integer> treeMap = new TreeMap<String, Integer>();
    ```
        
    - Map 인터페이스 타입 변수에 대입해도 되지만 TreeMap 클래스 타입으로 대입한 이유는 특정 객체를 찾거나 범위 검색과 관련된 메소드를 사용하기 위해서이다.
    
    ### TreeMap이 가지고 있는 검색 관련 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | Map.Entry<K, V> | firstEntry() | 제일 낮은 Map.Entry를 리턴 |
    | Map.Entry<K, V> | lastEntry() | 제일 높은 Map.Entry를 리턴 |
    | Map.Entry<K, V> | lowerEntry(K key) | 주어진 키보다 바로 아래 Map.Entry 리턴 |
    | Map.Entry<K, V> | higherEntry(K key) | 주어진 키보다 바로 위 Map.Entry 리턴 |
    | Map.Entry<K, V> | floorEntry(K key) | 주어진 키와 동등한 키가 있으면 해당 Map.Entry를 리턴, 없다면 주어진 키 바로 아래의 Map.Entry를 리턴 |
    | Map.Entry<K, V> | ceilingEntry(K key) | 주어진 키와 동등한 키가 있으면 해당 Map.Entry를 리턴, 없다면 주어진 키 바로 위의 Map.Entry를 리턴 |
    | Map.Entry<K, V> | pollFirstEntry() | 제일 낮은 Map.Entry를 꺼내오고 컬렉션에서 제거 |
    | Map.Entry<K, V> | pollLastEntry() | 제일 높은 Map.Entry를 꺼내오고 컬렉션에서 제거 |
- ex) 특정 Map.Entry 찾기
    - 점수를 키로,이름을 값으로 해서 무작위로 저장하고 특정 `Map.Entry`를 찾는 방법을 보여준다.
    - TreeMapEx1.java
    
    ```java
    import java.util.Map;
    import java.util.TreeMap;
    
    public class TreeMapEx1 {
    
    	public static void main(String[] args) {
    		TreeMap<Integer, String> scores = new TreeMap<Integer, String>();
    		scores.put(new Integer(87), "ABC");
    		scores.put(new Integer(98), "DEF");
    		scores.put(new Integer(75), "GHI");
    		scores.put(new Integer(95), "JKL");
    		scores.put(new Integer(80), "MNO");
    		
    		Map.Entry<Integer, String> entry = null;
    		
    		entry = scores.firstEntry();
    		System.out.println("가장 낮은 점수 : " + entry.getKey() + " - " + entry.getValue() + "\n");
    		
    		entry = scores.lastEntry();
    		System.out.println("가장 높은 점수 : " + entry.getKey() + " - " + entry.getValue() + "\n");
    		
    		entry = scores.lowerEntry(new Integer(95));
    		System.out.println("95점 아래 점수 : " + entry.getKey() + " - " +  entry.getValue() + "\n");
    		
    		entry = scores.higherEntry(new Integer(95));
    		System.out.println("95점 위의 점수 : " + entry.getKey() + " - " + entry.getValue() + "\n");
    		
    		entry = scores.floorEntry(new Integer(95));
    		System.out.println("95점 이거나 바로 아래 점수 : " + entry.getKey() + " - " + entry.getValue() + "\n");
    		
    		entry = scores.ceilingEntry(new Integer(85));
    		System.out.println("85점 이거나 바로 위의 점수 : " + entry.getKey() + " - " + entry.getValue() + "\n");
    		
    		while(!scores.isEmpty()) {
    			entry = scores.pollFirstEntry();
    			System.out.println(entry.getKey() + " - " + entry.getValue() + 
    					"(남은 객체 수 : " + scores.size() + ")");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeMap/Untitled%201.png)
    
    ### TreeMap이 가지고 있는 정렬과 관련된 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | NavigableSet<K> | descendingKeySet() | 내림차순으로 정렬된 키의 NavigableSet을 리턴 |
    | NavigableMap<K, V> | descendingMap() | 내림차순으로 정렬된 Map.Entry의 NavigableMap을 리턴 |
    - `descendingKeySet()` 메소드는 내림차순으로 정렬된 키의 NavigableSet 객체를 리턴
    - `descendingMap()` 메소드는 내림차순으로 정렬된 NavigableMap 객체를 리턴
        - `firstEntry()`, `lastEntry()`, `lowerEntry()`, `higherEntry()`, `floorEntry()`, `ceilingEntry()` 메소드를 제공
        - 오름차순과 내림차순을 번갈아가며 정렬 순서를 바꾸는 `descendingMap()` 메소드 제공
        - 오름차순으로 정렬하고 싶을 경우, `descendingMap()` 메소드를 두 번 호출
        
    ```java
    NavigableMap<K, V> descendingMap = treeMap.descendingMap();
    NavigableMap<K, V> ascendingMap = descendingMap.descendingMap();
    ```
        
- ex) 객체 정렬
    - TreeMapEx2.java
    
    ```java
    import java.util.Map;
    import java.util.NavigableMap;
    import java.util.Set;
    import java.util.TreeMap;
    
    public class TreeMapEx2 {
    
    	public static void main(String[] args) {
    		TreeMap<Integer, String> scores = new TreeMap<Integer, String>();
    		scores.put(new Integer(87), "ABC");
    		scores.put(new Integer(98), "DEF");
    		scores.put(new Integer(75), "GHI");
    		scores.put(new Integer(95), "JKL");
    		scores.put(new Integer(80), "MNO");
    		
    		NavigableMap<Integer, String> descendingMap = scores.descendingMap();
    		Set<Map.Entry<Integer, String>> descendingEntrySet = descendingMap.entrySet();
    		for(Map.Entry<Integer, String> entry : descendingEntrySet) {
    			System.out.print(entry.getKey() + " - " + entry.getValue() + " ");
    		}
    		System.out.println();
    		
    		NavigableMap<Integer, String> ascendingMap = descendingMap.descendingMap();
    		Set<Map.Entry<Integer, String>> ascendingEntrySet = ascendingMap.entrySet();
    		for(Map.Entry<Integer, String> entry : ascendingEntrySet) {
    			System.out.print(entry.getKey() + " - " + entry.getValue() + " ");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeMap/Untitled%202.png)
    
    ### TreeMap이 가지고 있는 범위 검색과 관련된 메소드
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeMap/Untitled%203.png)
    
    - `subMap()` 메소드의 사용 방법
        - 네 개의 매개 변수가 존재
            - 시작 키와 끝 키, 그리고 이 키들의 Map.Entry를 포함할지 여부의 boolean 값
        
        ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeMap/Untitled%204.png)
        
- ex) 키로 정렬하고 범위 검색
    - 영어 단어와 페이지 정보를 무작위로 TreeMap에 저장한 후 알파벳 c ~ f 사이의 단어를 검색
    - TreeMapEx3.java
    
    ```java
    import java.util.Map;
    import java.util.NavigableMap;
    import java.util.TreeMap;
    
    public class TreeMapEx3 {
    
    	public static void main(String[] args) {
    		TreeMap<String, Integer> treeMap = new TreeMap<String, Integer>();
    		treeMap.put("apple", new Integer(10));
    		treeMap.put("forever", new Integer(60));
    		treeMap.put("description", new Integer(40));
    		treeMap.put("ever", new Integer(50));
    		treeMap.put("zoo", new Integer(10));
    		treeMap.put("base", new Integer(20));
    		treeMap.put("guess", new Integer(70));
    		treeMap.put("cherry", new Integer(30));
    		
    		System.out.println("[ c ~ f 사이의 단어 검색 ]");
    		NavigableMap<String, Integer> rangeMap = treeMap.subMap("c", true, "f", true);
    		for(Map.Entry<String, Integer> entry : rangeMap.entrySet()) {
    			System.out.println(entry.getKey() + " - " + entry.getValue() + " 페이지");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeMap/Untitled%205.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판