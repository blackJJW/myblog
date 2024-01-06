---
title: "[Java]  List 컬렉션, Vector"
description: ""
date: "2022-11-11T21:50:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- ArrayList와 동일한 내부 구조
- Vector를 생성하기 위해서는 저장할 객체 타입을 타입 파라미터로 표기하고 기본 생성자를 호출하면 된다.
    
    ```java
    List<E> list = new Vector<E>();
    ```
    
- ArrayList와 다른 점은 Vector는 동기화된(synchronized) 메소드로 구성되어 있기 때문에 멀티 스레드가 동시에 이 메소드들을 실행 불가
    - 하나의 스레드가 실행을 완료해야만 다른 스레드를 실행 가능
    - 그래서 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제 가능
        - 이것을 스레드가 안전(Thread Safe)하다라고 말한다.
    
    ![Untitled](/images/lang_java/collectionFramework/List_컬렉션/Vector/Untitled.png)
    
- ex) Board 객체를 저장하는 Vector
    - Vector를 이용해서 Board 객체를 추가, 삭제, 검색
    - VectorEx.java
    
    ```java
    import java.util.List;
    import java.util.Vector;
    
    public class VectorEx {
    
    	public static void main(String[] args) {
    		List<Board> list = new Vector<Board>();
    		
    		// Board 객체를 저장
    		list.add(new Board("title1", "content1", "writer1"));
    		list.add(new Board("title2", "content2", "writer2"));
    		list.add(new Board("title3", "content3", "writer3"));
    		list.add(new Board("title4", "content4", "writer4"));
    		list.add(new Board("title5", "content5", "writer5"));
    		
    		list.remove(2); // 2번 인덱스 객체(title3) 삭제(뒤의 인덱스는 1씩 앞으로 당겨짐)
    		list.remove(3); // 3번 인덱스 객체(title5) 삭제
    		
    		for(int i = 0; i < list.size(); i++) {
    			Board board = list.get(i);
    			System.out.println(board.subject + "\t" + board.content + "\t" + board.writer);
    		}
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/List_컬렉션/Vector/Untitled%201.png)
    
- ex) 게시물 정보 객체
    - Board.java
    
    ```java
    public class Board {
    	String subject;
    	String content;
    	String writer;
    	
    	public Board(String subject, String content, String writer) {
    		this.subject = subject;
    		this.content = content;
    		this.writer = writer;
    	}
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판