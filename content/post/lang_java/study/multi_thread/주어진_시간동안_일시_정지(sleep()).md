---
title: "[Java] 스레드 상태 제어, 주어진 시간동안 일시 정지(sleep())"
description: ""
date: "2022-10-08T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 실행 중인 스레드를 일정 시간 멈추게 하고 싶다면 Thread 클래스의 정적 메소드인 `sleep()`을 사용하면 된다.
- Thread.sleep() 메소드를 호출한 스레드는 주어진 시간 동안 일시 정지 상태가 되고, 다시 실행 대기 상태로 돌아간다.
    
    ```java
    try {
    	Thread.sleep(1000);
    } catch(InterruptedException e){
    	// interrupt() 메소드가 호출되면 실행
    }
    ```
    
    - 매개값에는 얼마 동안 일시 정지 상태로 있을 것인지, 밀리세컨드(1/1000) 단위로 시간을 준다.
        - 1000을 주면 스레드는 1초가 경과할 동안 일시 정지 상태
    - 일시 정지 상태에서 주어진 시간이 되기 전에 `interrupt()` 메소드가 호출되면 InterruptedException이 발생하기 때문에 예외 처리가 필요
- ex) 3초 주기로 10번 비프음 발생
    - SleepEx.java
    
    ```java
    import java.awt.Toolkit;
    
    public class SleepEx {
    
    	public static void main(String[] args) {
    		Toolkit toolkit = Toolkit.getDefaultToolkit();
    		for(int i = 0; i < 10; i++) {
    			toolkit.beep();
    			try {
    				Thread.sleep(3000); // 3초 동안 메인 스레드를 일시 정지 상태로 만듦
    			}catch(InterruptedException e) {
    				
    			}
    		}
    
    	}
    
    }
    ```
    
    - try 문에서 메인 스레드를 3초동안 일시 정지 상태로 보낸다.
    - 3초가 지나면 다시 실행 준비 상태로 돌아온다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판