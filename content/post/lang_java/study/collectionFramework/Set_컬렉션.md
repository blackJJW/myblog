---
title: "[Java]  Set 컬렉션"
description: ""
date: "2022-11-12T12:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- List 컬렉션은 저장 순서 유지
    - Set 컬렉션은 저장 순서가 유지되지 않는다.
- 객체를 중복해서 저장 불가
- 하나의 null만 저장 가능
    
    ![Untitled](/images/lang_java/collectionFramework/Set_컬렉션/Untitled.png)
    
    - 순서와 상관없음
    - 중복 허용 불가
    - 저장할 때 순서와 찾을 때의 순서가 다를 수도 있음
- Set 컬렉션에는 HashSet, LinkedHashSet, TreeSet 등이 존재
    
    ### Set 컬렉션에서 공통적으로 사용 가능한 Set 인터페이스의 메소드
    
    - 인덱스로 관리하지 않기 때문에 인덱스를 매개값으로 갖는 메소드가 없다.
    - 객체 추가 기능
        
        
        | 메소드 | 설명 |
        | --- | --- |
        | boolean add(E e) | 주어진 객체를 저장, 객체가 성공적으로 저장되면 true를 리턴, 중복 객체면 false를 리턴 |
    - 객체 검색 기능
        
        
        | 메소드 | 설명 |
        | --- | --- |
        | boolean contains(Object o) | 주어진 객체가 저장되어 있는지 여부 |
        | boolean isEmpty() | 컬렉션이 비어 있는지 조사 |
        | Iterator<E> iterator() | 저장된 객체를 한 번씩 가져오는 반복자 리턴 |
        | int size() | 저장되어 있는 전체 객체 수 리턴 |
    - 객체 삭제 기능
        
        
        | 메소드 | 설명 |
        | --- | --- |
        | void clear() | 저장된 모든 객체를 삭제 |
        | boolean remove(Object) | 주어진 객체를 삭제 |
    - 메소드의 매개 변수 타입과 리턴 타입에 E라는 타입 파라미터가 존재
        - Set 인터페이스가 제네릭 타입이기 때문
        - 구체적인 타입은 구현 객체를 생성할 때 결정
    - 객체 추가는 `add()` 메소드를 사용
    - 객체 삭제는 `remove()` 메소드를 사용
- Set 컬렉션에 저장되는 구체적인 타입을 String으로 정해놓고, String 객체를 저장하고 삭제하는 방법
    
    ```java
    Set<String> set = ...;
    set.add("ABC");    // 객체 추가
    set.add("DEF");
    set.remove("ABC"); // 객체 삭제
    ```
    
- Set 컬렉션은 인덱스로 객체를 검색해서 가져오는 메소드가 없다.
    - 대신, 전체 객체를 대상으로 한번씩 반복해서 가져오는 반복자(Iterator)를 제공
    - 반복자는 Iterator 인터페이스를 구현한 객체
        - `iterator()` 메소드를 호출하면 얻을 수 있다.
    
    ```java
    Set<String> set = ...;
    Iterator<String> iterator = set.iterator();
    ```
    
    ### Iterator 인터페이스에 선언된 메소드
    
    | 리턴 타입 | 메소드명 | 설명 |
    | --- | --- | --- |
    | boolean | hasNext() | 가져올 객체가 있으면 true를 리턴, 없으면 false를 리턴 |
    | E | next() | 컬렉션에서 하나의 객체를 가져온다. |
    | void | remove() | Set 컬렉션에서 객체를 제거 |
    - Iterator에서 하나의 객체를 가져올 때는 `next()` 메소드를 사용
        - `next()` 메소드를 사용하기 전에 먼저 가져올 객체가 있는지 확인하는 것이 좋다.
    - hasNext() 메소드는 가져올 객체가 있으면 true를 리턴
        - 더이상 가져올 객체가 없다면 false를 리턴
        - true가 리턴될 때 `next()` 메소드를 사용해야 한다.
- Set 컬렉션에서 String 객체들을 반복해서 하나씩 가져오는 코드
    
    ```java
    Set<String> set = ...;
    Iterator<String> iterator = set.iterator();
    
    // 저장된 객체 수만큼 루핑
    while(iterator.hasNext()) {
    	// String 객체 하나를 가져옴
    	String str = iterator.next();
    }
    ```
    
- Iterator를 사용하지 않더라도 향상된 for문을 이용해서 전체 객체를 대상으로 반복 가능
    
    ```java
    Set<String> set = ...;
    
    // 저장된 객체 수 만큼 루핑 
    for(String str : set) {
    }
    ```
    
- Set 컬렉션에서 Iterator의 `next()` 메소드로 가져온 객체를 제거하고 싶을 경우
    - `remove()` 메소드를 호출하면 된다.
    - Iterator의 메소드이지만, 실제 Set 컬렉션에서 객체가 제거됨을 알아야 한다.
    - ex) Set 컬렉션에서 “ABC”를 제거
    
    ```java
    while(iterator.hasNext()) {
    	String str = iterator.next();
    	if(str.equals("ABC") {
    		iterator.remove();
    	}
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판