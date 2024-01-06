---
title: "[Java] Object 클래스, 객체 소멸자(finalize())"
description: ""
date: "2022-09-19T22:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 참조하지 않는 배열이나 객체는 Garbage Collector가 힙 영역에서 자동적으로 소멸시킨다.
- Garbage Collector는 객체를 소멸하기 직전에 마지막으로 객체의 소멸자(`finalize()`)를 실행시킨다.
- 소멸자는 Object의 `finalize()` 메소드를 의미
    - 기본적으로 실행 내용이 없다.
- 객체가 소멸되기 전에 마지막으로 사용했던 자원(데이터 연결, 파일 등)을 닫고 싶거나, 중요한 데이터를 저장하고 싶을 경우
    - Object의 `finalize()`를 재정의 가능
- ex) `finalize()` 메소드 재정의
    - `finalize()` 메소드가 실행되면 번호를 출력하게 해서 어떤 객체가 소멸되는지 확인 가능
    - Counter.java
    
    ```java
    public class Counter {
    
    	private int no;
    	
    	public Counter(int no) {
    		this.no = no;
    	}
    	
    	@Override
    	protected void finalize() throws Throwable{
    		System.out.println(no + "번 객체의 finalize()가 실행됨");
    	}
    }
    ```
    
- ex) `finalize()` 메소드 실행 확인
    - 한두 개의 객체를 garbage로 만들었다고 해서 Garbage Collector가 실행되는 것은 아니기 때문에 반복해서 객체를 생성하고 garbage로 만듦
    - 반복할 때마다 `System.gc()`를 호출해서 Garbage Collector를 가급적 빨리 실행하라고 JVM에게 요청
    - finalizeEx.java
    
    ```java
    public class FinalizeEx {
    
    	public static void main(String[] args) {
    		Counter counter = null;
    		for(int i = 1; i <= 50; i++) {
    			counter = new Counter(i);
    			
    			counter = null; // Counter 객체를 garbage로 만듦
    			
    			System.gc(); // Garbage collector 요청
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_소멸자(finalize())/Untitled.png)
    
    - 실행 결과를 보면, 순서대로 소멸시키지 않고, 무작위로 소멸시키는 것을 확인
    - 전부 소멸시키는 것이 아니라 메모리의 상태를 보고 일부만 소멸
    - 예제에는 `System.gc()`로 Garbage Collector를 실행 요청하였으나, Garbage Collector는 메모리가 부족할 때 그리고CPU가 한가할 때에 JVM에 자동 실행된다.
        - finalize() 메소드가 호출되는 시점이 명확하지 않음
- 프로그램이 종료될 때 즉시 자원을 해제하거나 즉시 데이터를 최종 저장해야 할 경우
    - 일반 메소드에서 작성하고 프로그램이 종료될 때 명시적으로 메소드를 호출하는 것이 좋다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판