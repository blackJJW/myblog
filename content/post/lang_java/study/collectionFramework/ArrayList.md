---
title: "[Java]  List 컬렉션, ArrayList"
description: ""
date: "2022-11-11T21:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- ArrayList는 List 인터페이스의 구현 클래스
    - ArrayList에 객체를 추가하면 객체가 인덱스로 관리된다.
    - 일반 배열과 ArrayList는 인덱스로 객체를 관리하다는 점에서 유사
        - 배열은 생성할 때 크기가 고정, 사용 중 크기를 변경 불가
        - ArrayList는 저장 용량(capacity)을 초과한 객체들이 들어오면 자동적으로 저장 용량(capacity)이 늘어난다.
    
    ### ArrayList 객체의 내부 구조
    
    ![Untitled](/images/lang_java/collectionFramework/List_컬렉션/ArrayList/Untitled.png)
    
- ArrayList를 생성하기 위해서는 저장할 객체 타입을 파라미터로 표기하고 기본 생성자를 호출하면 된다.
    - ex) String을 저장하는 ArrayList는 다음과 같이 생성
    
    ```java
    List<String> list = new ArrayList<String>();
    ```
    
- 기본 생성자로 ArrayList 객체를 생성하면 내부에 10개의 객체를 저장할 수 있는 초기 용량(capacity)을 가지게 된다.
    - 저장되는 객체 수가 늘어나면 용량이 자동으로 증가
    - 처음부터 용량을 크게 잡고 싶다면 용량의 크기를 매개값으로 받는 생성자를 이용하면 된다.
    
    ```java
    List<String> list = new ArrayList<String>(30);
                        // String 객체 30개를 저장할 수 있는 용량을 가짐
    ```
    
- 자바 4 이전까지는 타입 파라미터가 없기 때문에 다음과 같이 ArrayList 객체를 생성
    - 이렇게 생성된 ArrayList는 모든 종류의 객체를 저장 가능
    - 그 이유는 객체가 저장될 때 Object 타입으로 변환되어 저장되기 때문
    
    ```java
    List list = new ArrayList();
    ```
    
    - 모든 종류의 객체를 저장할 수 있다는 장점
    - 저장할 때 Object로 변환하고, 찾아올 때 원래 타입으로 변환해야 하므로 실행 성능에 좋지 못한 영향을 미친다.
- 일반적으로 컬렉션에는 단일 종류의 객체들만 저장
    - 자바 5부터 제네릭을 도입하여 ArrayList 객체를 생성할 때 타입 파라미터로 저장할 객체의 타입을 지정함으로써 불필요한 타입 변환을 하지 않도록 함
    
    ### 자바 4 이전과 자바 5 이후의 차이점
    
    - 자바 4 이전
        
        ```java
        List list = new ArrayList(); // 컬렉션 생성
        list.add("ABC");             // 컬렉션에 객체를 추가
        Object obj = list.get(0);    // 컬렉션에서 객체를 검색
        String name = (String) obj;  // 타입 변환 후 홍길동을 얻을 수 있음
        ```
        
    - 자바 5 이후
        
        ```java
        list<String> list = new ArrayList<String>();// 컬렉션 생성
        list.add("ABC");                            // 컬렉션에 객체를 추가
        String name = list.get(0);                 // 컬렉션에서 객체 검색, ABC를 바로 얻음
        ```
        
- ArrayList에 객체를 추가하면 인덱스 0부터 차례대로 저장
- ArrayList에서 특정 인덱스의 객체를 제거하면 바로 뒤 인덱스로부터 마지막 인덱스까지 모두 앞으로 1씩 당겨진다.
    - 마찬가지로 특정 인덱스에 객체를 삽입하면 해당 인덱스부터 마지막 인덱스까지 모두 1씩 밀려난다.
    - ex) 4번 인덱스가 제거되었을 때 5번 인덱스부터 모두 앞으로 1씩 당겨진다.
        
        ![Untitled](/images/lang_java/collectionFramework/List_컬렉션/ArrayList/Untitled%201.png)
        
    - 따라서 빈번한 객체와 삽입이 일어나는 곳에서는 ArrayList를 사용하는 것이 바람직하지 않다.
        - 이런 경우라면 LinkedList를 사용하는 것이 좋다.
        - 인덱스 검색이나, 맨 마지막에 객체를 추가하는 경우에는 ArrayList가 더 좋은 성능을 발휘한다.
- ex) String 객체를 저장하는 ArrayList
    - ArrayList에 String 객체를 추가, 검색, 삭제하는 방법을 보여줌
    - ArrayListEx.java
    
    ```java
    import java.util.*;
    
    public class ArrayListEx {
    
    	public static void main(String[] args) {
    		List<String> list = new ArrayList<String>();
    		
    		list.add("Java"); // String 객체를 저장
    		list.add("JDBC");
    		list.add("Servlet/JSP");
    		list.add(2, "Database");
    		list.add("iBATIS");
    		
    		int size = list.size(); // 저장된 총 객체 수 얻기
    		System.out.println("총 객체 수 : " + size);
    		System.out.println();
    		
    		String skill = list.get(2); // 2번 인덱스의 객체 얻기
    		System.out.println("2 : " + skill);
    		System.out.println();
    		
    		for(int i = 0; i < list.size(); i++) { // 저장된 총 객체 수만큼 루핑
    			String str = list.get(i);
    			System.out.println(i + " : " + str);
    		}
    		System.out.println();
    		
    		list.remove(2); // 2번 인덱스 객체(Database) 삭제됨
    		list.remove(2); // 2번 인덱스 객체(Servlet/JSP) 삭제됨
    		list.remove("iBATIS");
    		
    		for(int i = 0; i < list.size(); i++) { // 저장된 총 객체 수만큼 루핑
    			String str = list.get(i);
    			System.out.println(i + " : " + str);
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/List_컬렉션/ArrayList/Untitled%202.png)
    
- ArrayList를 생성하고 런타임 시 필요에 의해 객체들을 추가하는 것이 일반적
    - 고정된 객체들로 구성된 List를 생성할 때도 있다.
    - 이런 경우 `Arrays.asList(T… a)` 메소드를 사용하는 것이 간편
    
    ```java
    List<T> list = Arrays.asList(T... a);
    ```
    
    - T 타입 파라미터에 맞게 `asList()`의 매개값을 순차적으로 입력하거나, `T[]` 배열을 매개값으로 주면 된다.
- ex) `Arrays.asList()` 메소드
    - ArraysAsListEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    
    public class ArraysAsListEx {
    
    	public static void main(String[] args) {
    		List<String> list1 = Arrays.asList("A", "B", "C");
    		
    		for(String name : list1) {
    			System.out.println(name);
    		}
    		
    		List<Integer> list2 = Arrays.asList(1, 2, 3);
    		for(int value : list2) {
    			System.out.println(value);
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/List_컬렉션/ArrayList/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판