---
title: "[Java] 검색 기능을 강화시킨 컬렉션, Comparable과 Comparator"
description: ""
date: "2022-11-19T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- TreeSet의 객체와 TreeMap의 키는 저장과 동시에 자동 오름차순으로 정렬
    - 숫자(Integer, Double) 타입일 경우에는 값을 정렬
    - 문자열(String) 타일일 경우에는 유니코드로 정렬
- TreeSet과 TreeMap은 정렬을 위해 `java.lang.Comaprable`을 구현한 객체를 요구
    - Integer, Double, String은 모두 Comparable 인터페이스를 구현
- 사용자 정의 클래스도 Comparable을 구현한다면 자동 정렬이 가능
    - Comparable에는 `compareTo()` 메소드가 정의
        - 사용자 정의 클래스에서는 이 메소드를 오버라이딩하여 다음과 같이 리턴값을 만들어 내야 한다.
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | int | compareTo(T o) | 주어진 객체와 같으면 0을 리턴, 적으면 음수를 리턴, 크면 양수를 리턴 |
- ex) Comparable 구현 클래스
    - 나이를 기준으로 Person 객체를 오름차순으로 정렬하기 위해 Comparable 인터페이스를 구현
    - 나이가 적을 때는 -1을, 동일한 경우는 0을, 클 경우는 1을 리턴하도옥 `compareTo()` 메소드를 재정의
    - Person.java
    
    ```java
    public class Person implements Comparable<Person>{
    	public String name;
    	public int age;
    	
    	public Person(String name, int age) {
    		this.name = name;
    		this.age = age;
    	}
    	
    	@Override
    	public int compareTo(Person o) {
    		if(age < o.age) return -1;
    		else if(age == o.age) return 0;
    		else return 1;
    	}
    }
    ```
    
- ex) 사용자 정의 객체를 나이 순으로 정렬
    - ComparableEx.java
    
    ```java
    import java.util.Iterator;
    import java.util.TreeSet;
    
    public class ComaprableEx {
    
    	public static void main(String[] args) {
    		TreeSet<Person> treeSet = new TreeSet<Person>();
    		
    		// 저장될 때 나이 순으로 저장
    		treeSet.add(new Person("ABC", 45));
    		treeSet.add(new Person("DEF", 25));
    		treeSet.add(new Person("GHI", 31));
    		
        // 왼쪽 마지막 노드에서 오른쪽 마지막 노드까지 반복해서 가져오기
        // 오름차순
    		Iterator<Person> iterator = treeSet.iterator();
    		while(iterator.hasNext()) {
    			Person person = iterator.next();
    			System.out.println(person.name + " : " + person.age);
    		}
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/Comparable과_Comparator/Untitled.png)
    
- TreeSet의 객체와  TreeMap의 키가 Comaprable을 구현하고 있지 않을 경우에는 저장하는 순간 `ClassCastException`이 발생
- Comparable 비구현 객체를 정렬하는 방법
    - TreeSet 또는 TreeMap 생성자의 매개값으로 정렬자(Comparator)를 제공하면 Comparable 비구현 객체도 정렬 가능
    
    ```java
    TreeSet<E> treeSet = new TreeSet<E>( new AscendingComparator() );
                                           // 오름차순 또는 내림차순 정렬자
    TreeMap<K, V> treeMap = new TreeMap<K, V>( new DescendingComparator() );
    ```
    
    - 정렬자는 Comparator 인터페이스를 구현한 객체
    
    ### Comparator 인터페이스에 정의된 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | int | compare(T o1, T o2) | o1과 o2가 동등하면 0을 리턴, o1이 o2보다 앞에 오게 하려면 음수를 리턴, o1이 o2보다 뒤에 오게 하려면 양수를 리턴 |
    - compare() 메소드는 비교하는 두 객체가 동등하다면 0, 비교하는 값보다 앞에 오게 하려면 음수, 뒤에 오게 하려면 양수를 리턴하도록 구현
- ex) Fruit의 내림차순 정렬자
    - 가격을 기준으로 Fruit 객체를 내림차순으로 정렬시키는 정렬자
    - DescendingComparator.java
    
    ```java
    import java.util.Comparator;
    
    public class DescendingComparator implements Comparator<Fruit>{
    	@Override
    	public int compare(Fruit o1, Fruit o2) {
    		if(o1.price < o2.price) return 1; // 가격이 적을 경우 뒤에 오게 함
    		else if(o1.price == o2.price) return 0;
    		else return -1; // 가격이 클 경우 앞에 오게 함
    	}
    }
    ```
    
- ex) Comparable을 구현하지 않는 클래스
    - Fruit.java
    
    ```java
    public class Fruit {
    	public String name;
    	public int price;
    	
    	public Fruit(String name, int price) {
    		this.name = name;
    		this.price = price;
    	}
    }
    ```
    
- ex) 내림차순 정렬자를 사용하는 TreeSet
    - 내림차순 정렬자를 이용해서 TreeSet에 Fruit을 저장
    - 정렬자를 주지 않고 TreeSet에 저장하면 `ClassCastException` 예외가 발생
        - 정렬자를 TreeSet의 생성자에 주면 예외가 발생하지 않고 자동적으로 내림차순 정렬되는 것을 볼 수 있다.
    - ComparatorEx.java
    
    ```java
    import java.util.Iterator;
    import java.util.TreeSet;
    
    public class ComparatorEx {
    
    	public static void main(String[] args) {
    		/*
    		 * TreeSet<Fruit> treeSet = new TreeSet<Fruit>();
    		 * // Fruit이 Comparable을 구현하지 않았기 때문에 예외 발생
    		 * treeSet.add(new Fruit("grape", 3000));
    		 * treeSet.add(new Fruit("water melon", 10000));
    		 * treeSet.add(new Fruit("strawberry", 6000)); 
    		 * */
    		
    		TreeSet<Fruit> treeSet = new TreeSet<Fruit>(new DescendingComparator());
    		                                            // 내림차순 정렬자 제공
    		// 저장될 때 가격을 기준으로 내림차순 정렬됨
    		treeSet.add(new Fruit("grape", 3000));
    		treeSet.add(new Fruit("watermelon", 10000));
    		treeSet.add(new Fruit("strawberry", 6000));
    		
    		Iterator<Fruit> iterator = treeSet.iterator();
    		while(iterator.hasNext()) {
    			Fruit fruit = iterator.next();
    			System.out.println(fruit.name + " : " + fruit.price);
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/검색_기능을_강화시킨_컬렉션/Comparable과_Comparator/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판