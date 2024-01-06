---
title: "[Java]  List 컬렉션"
description: ""
date: "2022-11-10T22:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 객체를 일렬로 늘어놓은 구조
- 객체를 인덱스로 관리하기 때문에 객체를 저장하면 자동 인덱스가 부여
    - 인덱스로 객체를 검색, 삭제할 수 있는 기능을 제공
- 객체 자체를 저장하는 것이 아니라 객체의 번지를 참조
    
    ![Untitled](/images/lang_java/collectionFramework/List_컬렉션/Untitled.png)
    
    - 동일한 객체를 중복 저장 가능
        - 이 경우 동일한 번지가 참조된다.
    - null도 저장 가능
        - 이 경우 해당 인덱스는 객체를 참조하지 않는다.
- List 컬렉션에는 ArrayList, Vector, `LinkedList` 등이 존재
    
    ### List 컬렉션에서 공통적으로 사용 가능한 List 인터페이스의 메소드
    
    - 객체 추가
        
        
        | 메소드 | 설명 |
        | --- | --- |
        | boolean add(E e) | 주어진 객체를 맨 끝에 추가 |
        | void add(int index, E element) | 주어진 인덱스에 객체를 추가 |
        | E set(int index, E element) | 주어진 인덱스에 저장된 객체를 주어진 객체로 바꿈 |
    - 객체 검색
        
        
        | 메소드 | 설명 |
        | --- | --- |
        | boolean contains(Object o) | 주어진 객체가 저장되어 있는지 여부 |
        | E get(int index) | 주어진 인덱스에 저장된 객체를 리턴 |
        | boolean isEmpty() | 컬렉션이 비어 있는지 조사 |
        | int size() | 저장되어 있는 전체 객체 수를 리턴 |
    - 객체 삭제
        
        
        | 메소드 | 설명 |
        | --- | --- |
        | void clear() | 저장된 모든 객체를 삭제 |
        | E remove(int index) | 주어진 인덱스에 저장된 객체를 삭제 |
        | boolean remove(Object o) | 주어진 객체를 삭제 |
    - 메소드의 매개 변수 타입과 리턴 타입에 `E`라는 타입 파라미터가 존재
        - List 인터페이스가 제네릭 타입이기 때문
    - 구체적인 타입은 구현 객체를 생성할 때 결정
    - 객체 추가는 `add()` 메소드 사용
    - 객체를 찾을 때는 `get()` 메소드 사용
    - 객체 삭제는 `remove()` 메소드를 사용
- List 컬렉션에 저장되는 구체적인 타입을 String으로 정하고, 추가, 삽입, 찾기, 삭제하는 방법
    
    ```java
    List<String> list = ...;
    list.add("ABC");          // 맨끝에 객체 추가
    list.add(1, "DEF");       // 지정된 인덱스에 객체 삽입
    String str = list.get(1); // 인덱스로 객체 찾기
    list.remove(0);           // 인덱스로 객체 삭제
    list.remove("DEF");       // 객체 삭제
    ```
    
- 전체 객체를 대상으로 하나씩 반복해서 저장된 객체를 얻고 싶을 경우
    - for문을 사용
    
    ```java
    List<String> list = ...;
    for(int i = 0; i < list.size(); i++) { // 저장된 총 객체 수만큼 루핑
    	String str = list.get(i);
                 // i 인덱스에 저장된 String 객체를 가져옴
    }
    ```
    
    - 인덱스가 필요 없을 경우, 향상된 for문을 사용
    
    ```java
    for(String str : list) { 저장된 총 객체 수만큼 루핑 
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판