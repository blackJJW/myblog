---
title: "[Java] 스레드 상태 제어, 스레드의 안전한 종료(stop 플래그, interrupt())"
description: ""
date: "2022-10-11T23:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 스레드는 자신의 `run()` 메소드가 모두 실행되면 자동적으로 종료된다.
- 경우에 따라서는 실행 중인 스레드를 즉시 종료할 필요가 있다.
    - Thread는 스레드를 즉시 종료시키기 위해서 `stop()` 메소드를 제공하지만, deprecated됨
        - `stop()` 메소드로 스레드를 갑자기 종료하게 되면 스레드가 사용 중이던 자원들이 불안정한 상태로 남겨지기 때문
            - 자원 : 네트워크 연결, 파일 등

## 스레드를 즉시 종료시키기 위한 최선의 방법

---

### 1. stop 플래그를 이용하는 방법

- 스레드는 run() 메소드가 끝나면 자동적으로 종료되므로, run() 메소드가 정상적으로 종료되도록 유도하는 것이 최선의 방법
- stop 플래그를 이용해서 `run()` 메소드의 종료를 유도
    
    ```java
    public class XXXThread extends Thread {
    	private boolean stop; // stop 플래그 필드
    	
    	public void run() {
    		while( !stop ){ // stop이 true가 되면 run()이 종료
    			스레드가 반복 실행하는 코드;
    		}
    		// 스레드가 사용한 자원 정리
    
    	}
    }
    ```
    
    - stop 필드가 false일 경우에는 while문의 조건식이 true가 되어 반복 실행한다.
    - stop 필드가 true일 경우에는 while문의 조건식이 false가 되어 while문을 빠져나온다.
        - 스레드가 사용한 자원을 정리하고, `run()` 메소드가 끝나게 됨으로써 스레드는 안전하게 종료된다.
- ex) 1초 후 출력 스레드를 중지시킴
    - PrintThread1을 실행한 후, 1초 후에 PrintThread1을 멈추도록 `setStop()` 메소드를 호출
    - StopFlagEx.java
    
    ```java
    public class StopFlagEx {
    
    	public static void main(String[] args) {
    		PrintThread1 printThread = new PrintThread1();
    		printThread.start();
    		
    		try { Thread.sleep(1000); } catch (InterruptedException e) {}
    		
    		printThread.setStop(true);
    
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/multi_thread/스레드의_안전한_종료(stop_플래그,_interrupt())/Untitled.png)
    
- ex) 무한 반복해서 출력하는 스레드
    - PrintThread1.java
    
    ```java
    public class PrintThread1 extends Thread{
    	private boolean stop;
    	
    	public void setStop(boolean stop) {
    		this.stop = stop;
    	}
    	
    	public void run() {
    		while(!stop) {
    			System.out.println("실행 중...");
    		}
    		
    		// stop이 true일 때
    		System.out.println("자원 정리");
    		System.out.println("실행 종료");
    	}
    }
    ```
    

### 2. `interrupt()` 메소드를 이용하는 방법

- `interrupt()` 메소드는 스레드가 일시 정지 상태에 있을 때 InterruptedException 예외를 발생시키는 역할을 한다.
    - 이것을 이용하면 `run()` 메소드를 정상 종료시킬 수 있다.
- 다음 그림과 같이 ThreadA가 ThreadB를 생성해서 `start()` 메소드로 ThreadB를 실행했다고 가정
    
    ![Untitled](/images/lang_java/multi_thread/스레드의_안전한_종료(stop_플래그,_interrupt())/Untitled%201.png)
    
    - ThreadA가 ThreadB의 `interrupt()` 메소드를 실행하게 되면
        - ThreadB가 `sleep()` 메소드로 일시 정지 상태가 될 때 ThreadB에서 InerruptedException이 발생하여 예외 처리(catch) 블록으로 이동
    - 결국 ThreadB는 while문을 빠져나와 `run()` 메소드를 정상 종료하게 된다.
- ex) 1초 후 출력 스레드를 중지시킴
    - PrintThread2를 실행한 후 1초 후에 PrintThread2를 멈추도록 `interrupt()` 메소드를 호출
    - InterruptEx.java
    
    ```java
    public class InterruptEx {
    
    	public static void main(String[] args) {
    		Thread thread = new PrintThread2();
    		thread.start();
    		
    		try { Thread.sleep(1000); } catch (InterruptedException e) {}
    		
    		// 스레드를 종료시키기 위해 InterruptedException을 발생시킴
    		thread.interrupt();
    
    	}
    }
    ```
    
- ex) 무한 반복해서 출력하는 스레드
    - PrintThread2.java
    
    ```java
    public class PrintThread2 extends Thread{
    	public void run() {
    		try {
    			while(true) {
    				System.out.println("실행 중...");
    				Thread.sleep(1); // InterruptedException 발생
    			}
    		} catch(InterruptedException e) {
    		}
    		
    		System.out.println("자원 정리");
    		System.out.println("실행 종료");
    	}
    }
    ```
    
    - 주목할 점은 스레드가 실행 대기 또는 실행 상태에 있을 때 `interrupt()` 메소드가 실행되면 즉시 InterruptedException 예외가 발생하지 않는다.
        - 스레드가 미래에 일시 정지 상태가 되면 InterruptedException 예외가 발생
        - 스레드가 일시 정지 상태가 되지 않으면 `interrupt()` 메소드 호출은 아무런 의미가 없다.
            - 그래서 짧은 시간이나마 일시 정지시키기 위해 `Thread.sleep(1)`을 사용
    
    ![Untitled](/images/lang_java/multi_thread/스레드의_안전한_종료(stop_플래그,_interrupt())/Untitled%202.png)
    
- 일시 정지를 만들지 않고도 `interrupt()` 호출 여부를 알 수 있는 방법이 존재
    - `interrupt()` 메소드가 호출되었다면 스레드의 `interrupted()`와 `isInterrupted()` 메소드는 true를 리턴
    - `interrupted()`는 정적 메소드로 현재 스레드가 interrupted되었는지 확인
    - `isInterrupted()`는 인스턴스 메소드로 현재 스레드가 interrupted되었는지 확인할 때 사용
    
    ```java
    boolean status = Thread.interrupted();
    boolean status = objThread.isInterrupted();
    ```
    
- ex) 무한 반복해서 출력하늠 스레드
    - PrintThread2를 수정
        - 일시 정지 코드인 `Thread.sleep(1)`을 사용하지 않음
        - `Thread.interrupted()`를 사용해서 PrintThread2의 `interrupt()`가 호출되었는지 확인한 다음 while문을 빠져나가도록 한다.
    
    ```java
    public class PrintThread2 extends Thread{
    	public void run() {
    		while(true) {
    			System.out.println("실행 중...");
    			if(Thread.interrupted()) {
    				break; // while문을 빠져나욤
    			}
    		}
    		
    		System.out.println("자원 정리");
    		System.out.println("실행 종료");
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/multi_thread/스레드의_안전한_종료(stop_플래그,_interrupt())/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판