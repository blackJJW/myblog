---
title: "[Java] 스레드 상태 제어, 다른 스레드에게 실행 양보(yield())"
description: ""
date: "2022-10-08T22:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 스레드가 처리하는 작업은 반복적인 실행을 위해 for문이나 while문을 포함하는 경우가 많다.
    - 가끔은 이 반복문들이 무의미한 반복을 하는 경우가 있다.
    
    ```java
    public void run(){
    	while(true) {
    		if(work) {
    			System.out.println("ThreadA 작업 내용");
    		}
    	}
    }
    ```
    
    - 스레드가 시작되어 `run()` 메소드를 실행하면 `while(true) {}` 블록을 무한 반복 실행
    - work의 값이 false일 경우, 그리고 work의 값이 false에서 true로 변경되는 시점이 불명확하다면
        - while 문은 어떠한 실행문도 실행하지 않고 무의미한 반복을 한다.
        - 이것보다는 다른 스레드에게 실행을 양보하고 자신은 실행 대기 상태로 가는 것이 전체 프로그램 성능에 도움이 된다.
            - 이런 기능을 위해서 스레드는 `yield()` 메소드를 제공
    - `yield()` 메소드를 호출한 스레드는 실행 대기 상태로 돌아가고 동일한 우선순위 또는 높은 우선순위를 갖는 다른 스레드가 실행 기회를 가질 수 있도록 해준다.
    
    ![Untitled](/images/lang_java/multi_thread/다른_스레드에게_실행_양보(yield())/Untitled.png)
    
- 다음 코드는 의미 없는 반복을 줄이기 위해 `yield()` 메소드를 호출해서 다른 스레드에게 실행 기회를 주도록 수정
    
    ```java
    public void run() {
    	while(true) {
    		if(work) {
    			System.out.println("ThreadA 작업 내용");
    		} else {
    			Thread.yield();
    		}
    	}
    }
    ```
    
- ex) 스레드 실행 양보 예제
    - 처음 실행 후 3초 동안은 ThreadA와 ThreadB가 번갈아가며 실행
    - 3초 뒤에 메인 스레드가 ThreadA의 work 필드를 false로 변경함으로써 ThreadA는 `yield()` 메소드를 호출
        - 이후 3초 동안에는 ThreadB가 더 많은 실행 기회를 얻게 된다.
    - 메인 스레드는 3초 뒤에 다시 ThreadA의 work 필드를 true로 변경해서 ThreadA와 ThreadB가 변갈아가며 실행
    - 메인 스레드는 3초 뒤에 ThreadA와 ThreadB의 stop 필드를 true로 변경해서 두 스레드가 반복 작업을 중지하고 종료하도록 한다.
    - YieldEx.java
    
    ```java
    public class YieldEx {
    
    	public static void main(String[] args) {
    		ThreadA threadA = new ThreadA();
    		ThreadB threadB = new ThreadB();
    		
    		// ThreadA, ThreadB 모두 실행
    		threadA.start();
    		threadB.start();
    		
    		try {Thread.sleep(3000);} catch (InterruptedException e) {}
    		// ThreadB만 실행
    		threadA.work = false;
    		
    		try {Thread.sleep(3000);} catch (InterruptedException e) {}
    		// ThreadA, ThreadB만 모두 실행
    		threadA.work = true;
    		
    		try {Thread.sleep(3000);} catch (InterruptedException e) {}
    		// ThreadA, ThreadB만 모두 종료
    		threadA.work = false;
    		threadB.work = false;
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/multi_thread/다른_스레드에게_실행_양보(yield())/Untitled%201.png)
    
    - ThreadA.java
        
        ```java
        public class ThreadA extends Thread{
        	public boolean stop = false; // 종료 플래그
        	public boolean work = true;  // 작업 진행 여부 플래그
        	
        	public void run() {
        		// stop이 true가 되면 while문 종료
        		while(stop) {
        			if(work) {
        				System.out.println("ThreadA 작업 내용");
        			} else {
        				// work가 false가 되면 다른 스레드에게 실행 양보
        				Thread.yield();
        			}
        		}
        		System.out.println("ThreadA 종료");
        	}
        
        }
        ```
        
    - ThreadB.java
        
        ```java
        public class ThreadB extends Thread{
        	public boolean stop = false; // 종료 플래그
        	public boolean work = true;  // 작업 진행 여부 플래그
        	
        	public void run() {
        		// stop이 true가 되면 while문 종료
        		while(stop) {
        			if(work) {
        				System.out.println("ThreadB 작업 내용");
        			} else {
        				// work가 false가 되면 다른 스레드에게 실행 양보
        				Thread.yield();
        			}
        		}
        		System.out.println("ThreadB 종료");
        	}
        
        }
        ```
      

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판