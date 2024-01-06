---
title: "[Java] LIFO와 FIFO 컬렉션, Stack"
description: ""
date: "2022-11-19T22:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Stack 클래스는 LIFO 자료구조를 구현한 클래스
    
    ![Untitled](/images/lang_java/collectionFramework/LIFO와_FIFO_컬렉션/Stack/Untitled.png)
    
    ### Stack 클래스의 주요 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | E | push(E item) | 주어진 객체를 스택에 넣는다. |
    | E | peek() | 스택의 맨 위 객체를 가져온다. 객체를 스택에서 제거하지 않는다. |
    | E | pop() | 스택의 맨 위 객체를 가져온다. 객체를 스택에서 제거한다. |
- Stack 객체를 생성하기 위해서는 저장할 객체 타입을 파라미터로 표기하고 기본 생성자를 호출하면 된다.
    
    ```java
    Stack<E> stack = new Stack<E>
    ```
    
- ex) 동전 클래스
    - 동전케이스를 Stack 클래스로 구현
    - Coin,java
    
    ```java
    public class Coin {
    	private int value;
    	
    	public Coin(int value) {
    		this.value = value;
    	}
    	
    	public int getValue() {
    		return value;
    	}
    }
    ```
    
- ex) Stack을 이용한 동전케이스
    - StackEx.java
    
    ```java
    import java.util.Stack;
    
    public class StackEx {
    
    	public static void main(String[] args) {
    		Stack<Coin> coinBox = new Stack<Coin>();
    		
    		// 동전을 끼움
    		coinBox.push(new Coin(100));
    		coinBox.push(new Coin(50));
    		coinBox.push(new Coin(500));
    		coinBox.push(new Coin(10));
    		
    		while(!coinBox.isEmpty()) { // 동전 케이스가 비었는지 확인
    			// 동전 케이스에서 제일 위의 동전 꺼내기
    			Coin coin = coinBox.pop();
    			System.out.println("꺼내온 동전 : " + coin.getValue() + " 원");
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/LIFO와_FIFO_컬렉션/Stack/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판