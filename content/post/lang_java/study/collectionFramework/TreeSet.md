---
title: "[Java] 검색 기능을 강화시킨 컬렉션, TreeSet"
description: ""
date: "2022-11-16T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- TreeSet은 이진 트리(bianary tree)를 기반으로한 Set 컬렉션
- 하나의 노드값인 value의 왼쪽과 오른쪽 자식 노드를 참조하기 위한 두 개의 변수로 구성
- TreeSet에 객체를 저장하면 자동으로 정렬
    - 부모값과 비교
        - 낮은 것은 왼쪽 자식 노드에 저장
        - 큰 것은 오른쪽 자식 노드에 저장
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeSet/Untitled.png)
    
- TreeSet을 생성하기위해서는 저장할 객체 타입을 파라미터로 표기하고 기본 생성자를 호출하면 된다.
    
    ```java
    TreeSet<E> treeSet = new TreeSet<E>();
    ```
    
    - String 객체를 저장하는 TreeSet은 다음과 같이 생성
    
    ```java
    TreeSet<String> treeSet = new TreeSet<String>();
    ```
    
- Set 인터페이스 타입 변수에 대입해도 되지만 TreeSet 클래스 타입으로 대입한 이유는 객체를 찾거나 범위 검색과 관련된 메소드를 사용하기 위해서이다.
    
    ### 검색 관련 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | E | first() | 제일 낮은 객체를 리턴 |
    | E | last() | 제일 높은 객체를 리턴 |
    | E | lower(E e) | 주어진 객체보다 바로 아래 객체를 리턴 |
    | E | higher(E e) | 주어진 객체보다 바로 위 객체를 리턴 |
    | E | floor(E e) | 주어진 객체와 동등한 객체가 있으면 리턴, 만약 없다면 주어진 객체의 바로 아래의 객체를 리턴 |
    | E | ceiling(E e) | 주어진 객체와 동등한 객체가 있으면 리턴, 만약 없다면 주어진 객체의 바로 위의 객체를 리턴 |
    | E | pollFirst() | 제일 낮은 객체를 꺼내오고 컬렉션에서 제거 |
    | E | pollLast() | 제일 높은 객체를 꺼내오고 컬렉션에서 제거 |
- ex) 특정 객체 찾기
    - 점수를 무작위로 저장하고 특정 점수를 찾는 방법
    - TreeSetEx1.java
    
    ```java
    import java.util.TreeSet;
    
    public class TreeSetEx1 {
    
    	public static void main(String[] args) {
    		TreeSet<Integer> scores = new TreeSet<Integer>();
    		scores.add(new Integer(87));
    		scores.add(new Integer(98));
    		scores.add(new Integer(75));
    		scores.add(new Integer(95));
    		scores.add(new Integer(80));
    		
    		Integer score = null;
    		
    		score = scores.first();
    		System.out.println("가장 낮은 점수 : " + score);
    		
    		score = scores.last();
    		System.out.println("가장 높은 점수 : " + score + '\n');
    		
    		score = scores.lower(new Integer(95));
    		System.out.println("95점 아래 점수 : " + score);
    		
    		score = scores.higher(new Integer(95));
    		System.out.println("95점 위의 점수 : " + score + '\n');
    		
    		score = scores.floor(new Integer(95));
    		System.out.println("95점 이거나 바로 아래 점수 : " + score);
    		
    		score = scores.ceiling(new Integer(85));
    		System.out.println("85점 이거나 바로 위의 점수 : " + score + '\n');
    		
    		while(!scores.isEmpty()) {
    			score = scores.pollFirst();
    			System.out.println(score + "(남은 객체 수 : " + scores.size() + ")");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeSet/Untitled%201.png)
    
- TreeSet이 가지고 있는 정렬과 관련된 메소드
    
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | Iterator<E> | descendingIterator() | 내림차순으로 정렬된 Iterator를 리턴 |
    | NavigableSet<E> | descendingSet() | 내림차순으로 정렬된 NavigableSet을 반환 |
    - `descendingIterator()` 메소드는 내림차순으로 정렬도니 Iterator 객체를 리턴
    - `descendingSet()` 메소드는 내림차순으로 정렬된 NavigableSet 객체를 리턴
        - NaviagableSet은 TreeSet과 마찬가지로 `first()`, `last()`, `lower()`, `higher()`, `floor()`, `ceiling()` 메소드를 제공
            - 정렬 순서를 바꾸는 `descendingSet()` 메소드 제공
        - 오름차순으로 정렬하고 싶은 경우, `descendingSet()` 메소드를 두 번 호출
    
    ```java
    Navigable<E> descendingSet = treeSet.descendingSet();
    Navigable<E> ascendingSet = descendingSet.descendingSet();
    ```
    
- ex) 객체 정렬하기
    - TreeSetEx2.java
    
    ```java
    import java.util.NavigableSet;
    import java.util.TreeSet;
    
    public class TreeSetEx2 {
    
    	public static void main(String[] args) {
    		TreeSet<Integer> scores = new TreeSet<Integer>();
    		scores.add(new Integer(87));
    		scores.add(new Integer(98));
    		scores.add(new Integer(75));
    		scores.add(new Integer(95));
    		scores.add(new Integer(80));
    		
    		NavigableSet<Integer> descendingSet = scores.descendingSet();
    		for(Integer score : descendingSet) {
    			System.out.print(score + " ");
    		}
    		System.out.println();
    		
    		NavigableSet<Integer> ascendingSet = descendingSet.descendingSet();
    		
    		for(Integer score : ascendingSet) {
    			System.out.print(score + " ");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeSet/Untitled%202.png)
    
- TreeSet이 가지고 있는 범위 검색과 관련된 메소드
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeSet/Untitled%203.png)
    
    ### `subSet()` 메소드의 사용 방법
    
    - 네 개의 매개 변수가 존재
        - 시작 객체와 끝 객체, 그리고 이 객체들을 포함할 지 여부의 boolean 값을 받는다.
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeSet/Untitled%204.png)
    
- ex) 영어 단어를 정렬하고, 범위 검색
    - TreeSetEx.java
    
    ```java
    import java.util.NavigableSet;
    import java.util.TreeSet;
    
    public class TreeSetEx3 {
    
    	public static void main(String[] args) {
    		TreeSet<String> treeSet = new TreeSet<String>();
    		treeSet.add("apple");
    		treeSet.add("forever");
    		treeSet.add("description");
    		treeSet.add("ever");
    		treeSet.add("zoo");
    		treeSet.add("base");
    		treeSet.add("guess");
    		treeSet.add("cherry");
    		
    		System.out.println("[ c-f 사이의 단어 검색 ]");
    		NavigableSet<String> rangeSet = treeSet.subSet("c", true, "f", true);
    		                                     // "c"<= 검색 범위 <= "f"
    		for(String word : rangeSet) {
    			System.out.println(word);
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/TreeSet/Untitled%205.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판