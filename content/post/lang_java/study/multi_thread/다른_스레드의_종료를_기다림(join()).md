---
title: "[Java] 스레드 상태 제어, 다른 스레드의 종료를 기다림(join())"
description: ""
date: "2022-10-08T23:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 스레드는 다른 스레드와 독립적으로 실행하는 것이 기본이지만
    - 다른 스레드가 종료될 때까지 기다렸다가 실행해야 하는 경우가 발생할 수도 있다.
    - ex) 계산 작업을 하는 스레드가 모든 계산 작업을 마쳤을 경우
        - 계산 결과값을 받아 이용하는 경우가 이에 해당
        - 이런 경우를 위해서 Thread는 `join()` 메소드를 제공
- ThreadA와 ThreadB의 join() 메소드를 호출하면 ThreadA는 ThreadB가 종료할 때까지 일시 정지 상태가 된다.
    - ThreadB의 run() 메소드가 종료되면 ThreadA는 일시 정지에서 풀려 다음 코드를 실행
    
    ![Untitled](/images/lang_java/multi_thread/다른_스레드의_종료를_기다림(join())/Untitled.png)
    
- ex) 1부터 100까지 합을 계산하는 스레드
    - 메인 스레드는 SumThread가 계산 작업을 모두 마칠 때까지 일시 정지 상태에 있다가 SumThread가 최종 계산된 결과값을 산출하고 종료하면 결과값을 받아 출력
    - SumThread.java
    
    ```java
    public class SumThread extends Thread{
    	private long sum;
    	
    	public long getSum() {
    		return sum;
    	}
    	
    	public void setSum(long sum) {
    		this.sum = sum;
    	}
    	
    	public void run() {
    		for(int i = 1; i <= 100; i++) {
    			sum += i;
    		}
    	}
    
    }
    ```
    
- ex) 다른 스레드가 종료될 때까지 일시 정지 상테 유지
    - JoinEx.java
    
    ```java
    public class JoinEx {
    
    	public static void main(String[] args) {
    		SumThread sumThread = new SumThread();
    		sumThread.start();
    		
    		try {
    			// sumThread가 종료될 때까지 메인 스레드를 일시 정지시킴
    			sumThread.join();
    		} catch (InterruptedException e) {
    		}
    		
    		System.out.println("1 ~ 100 합 : " + sumThread.getSum());
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/multi_thread/다른_스레드의_종료를_기다림(join())/Untitled%201.png)
    
    - `try - catch` 문을 주석 처리하고 실행 시 1 ~ 100까지의 합은 0이 나온다.
        
        ![Untitled](/images/lang_java/multi_thread/다른_스레드의_종료를_기다림(join())/Untitled%202.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판