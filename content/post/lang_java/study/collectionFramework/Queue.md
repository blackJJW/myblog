---
title: "[Java] LIFO와 FIFO 컬렉션, Queue"
description: ""
date: "2022-11-19T23:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Queue 인터페이스는 FIFO 자료구조에서 사용되는 메소드를 정의
    
    ![Untitled](/images/lang_java/collectionFramework/LIFO와_FIFO_컬렉션/Queue/Untitled.png)
    
    ### Queue 인터페이스에 정의되어 있는 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | boolean | offer(E e) | 주어진 객체를 넣는다. |
    | E | peek() | 객체 하나를 가져온다. 객체를 큐에서 제거하지 않는다. |
    | E | poll() | 객체 하나를 가져온다. 객체를 큐에서 제거한다. |
- Queue 인터페이스를 구현한 대표적인 클래스는 LinkedList
    - LinkedList는 List 인터페이스를 구현했기 때문에 List 컬렉션이기도 하다.
    - LinkedList 객체를 Queue 인터페이스 타입으로 변환하는 코드
    
    ```java
    Queue<E> queue = new LinkedList<E>();
    ```
    
- ex) Message 클래스
    - Queue를 이용해서 간단한 메시지 큐를 구현
    - 먼저 넣은 메시지가 반대쪽으로 먼저 나오기 때문에 넣은 순서대로 메시지가 처리
    - Message.java
    
    ```java
    public class Message {
    	public String command;
    	public String to;
    	
    	public Message(String command, String to) {
    		this.command = command;
    		this.to = to;
    	}
    }
    ```
    
- ex) Queue를 이용한 메시지 큐
    - QueueEx.java
    
    ```java
    import java.util.LinkedList;
    import java.util.Queue;
    
    public class QueueEx {
    
    	public static void main(String[] args) {
    		Queue<Message> messageQueue = new LinkedList<Message>();
    		
    		// 메시지 저장
    		messageQueue.offer(new Message("sendMail", "ABC"));
    		messageQueue.offer(new Message("sendSMS", "DEF"));
    		messageQueue.offer(new Message("sendKakaotalk", "GHI"));
    		
    		while(!messageQueue.isEmpty()) { // 메시지 큐가 비어있는지 확인
    			// 메시지 큐에서 한 개의 메시지 꺼냄
    			Message message = messageQueue.poll();
    			switch(message.command) {
    			case "sendMail":
    				System.out.println(message.to + " 님에게 메일을 전송");
    				break;
    			case "sendSMS":
    				System.out.println(message.to + " 님에게 SMS를 전송");
    				break;
    			case "sendKakaotalk":
    				System.out.println(message.to + " 님에게 Kakaotalk 전송");
    				break;
    			}
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/LIFO와_FIFO_컬렉션/Queue/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판