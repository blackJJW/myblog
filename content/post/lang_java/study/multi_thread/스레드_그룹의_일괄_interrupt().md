---
title: "[Java] 스레드 그룹의 일괄 interrupt()"
description: ""
date: "2022-10-15T21:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 스레드를 스레드 그룹에 포함시킬 경우의 이점
    - 스레드 그룹에서 제공하는 `interrupt()` 메소드를 이용하면 그룹 내에 포함된 모든 스레드를 interrupt 가능
    - ex) 10개의 스레드들을 모두 종료시키기 위해 각 스레드에서 `interrupt()` 메소드를 10번 호출할 수도 있다.
        - 이 스레드들이 같은 스레드 그룹에 소속되어 있을 경우
            - 스레드 그룹의 `interrupt()` 메소드를 한 번만 호출해주면 된다.
        - 이것이 가능한 이유는 스레드 그룹의 `interrupt()` 메소드는 포함된 모든 스레드의 `interrupt()` 메소드를 내부적으로 호출해주기 때문
        
        ![Untitled](/images/lang_java/multi_thread/스레드_그룹의_일괄_interrupt()/Untitled.png)
        
- 스레드 그룹의 `interrupt()` 메소드는 소속된 스레드의 `interrupt()` 메소드를 호출할 뿐 개별 스레드에서 발생하는 InterruptedException에 대한 예외 처리를 하지 않는다.
    - 안전한 종료를 위해서는 개별 스레드가 예외 처리를 해야 한다.
- 스레드 그룹에는 `interrupt()` 메소드 이외에도 `suspend()`, `resume()`, `stop()` 메소드들이 있다.
    - 모두 Deprecated되었다.
    - stop() 메소드를 호출하면 그룹에 포함된 모든 스레드들의 stop() 메소드가 일괄 호출되어 모든 스레드들을 쉽게 종료 가능
        - 스레드의 안전성 문제 때문에 가급적 사용하지 말아야 한다.
        - 대신 `interrupt()` 메소드로 스레드들을 안전하게 종료하도록 유도하는 것이 좋다.
- ThreadGroup이 가지고 있는 주요 메소드
    
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | int | activeCount() | 현재 그룹 및 하위 그룹에서 활동 중인 모든 스레드의 수를 리턴 |
    | int  | activeGroupCount() | 현재 그룹에서 활동 중인 모든 하위 그룹의 수를 리턴 |
    | void | checkAccess() | 현재 스레드가 스레드 그룹을 변경할 권한이 있는지 체크. 만약 권한이 없으면 SecurityException을 발생시킨다. |
    | void  | destroy() | 현재 그룹 및 하위 그룹을 모두 삭제. 단, 그룹 내에 포함된 모든 스레드들이 종료 상태가 되어야 한다. |
    | boolean | isDestroyed() | 현재 그룹이 삭제되었는지 여부를 리턴 |
    | int | getMaxPriority() | 현재 그룹에 포함된 스레드가 가질 수 있는 최대 우선순위를 리턴 |
    | void | setMaxPriority(int pri) | 현재 그룹에 포함된 스레드가 가질 수 있는 최대 우선순위를 설정 |
    | String  | getName() | 현재 그룹의 이름을 리턴 |
    | ThreadGroup | getParent() | 현재 그룹의 부모 그룹을 리턴 |
    | boolean | parentOf(ThreadGroup g) | 현재 그룹이 매개값으로 지정한 스레드 그룹의 부모인지 여부를 리턴 |
    | boolean | isDaemon() | 현재 그룹이 데몬 그룹인지 여부를 리턴 |
    | void | setDaemon(boolean daemon) | 현재 그룹을 데몬 그룹으로 설정 |
    | void  | list() | 현재 그룹에 포함된 스레드와 하위 그룹에 대한 정보를 출력 |
    | void  | interrupt() | 현재 그룹에 포함된 모든 스레드들을 interrupt한다. |
- ex) InterruptedException이 발생할 때 스레드가 종료되도록 함
    - 스레드 그룹을 생성, 정보를 출력
    - 3초 후 스레드 그룹의 `interrupt()` 메소드를 호출해서 스레드 그룹에 포함된 모든 스레드들을 종료
    - WorkThread.java
    
    ```java
    public class WorkThread extends Thread{
    	public WorkThread(ThreadGroup threadGroup, String threadName) {
    		// 스레드 그룹과 스레드 이름을 설정
    		super(threadGroup, threadName);
    	}
    	
    	@Override
    	public void run() {
    		while(true) {
    			try {
    				Thread.sleep(1000);
    			} catch(InterruptedException e) {
    				/* InterruptedException이 발생될 때
    				 * while문을 빠져나와 스레드를 종료시킴
    				 * */
    				System.out.println(getName() + "interrupted");
    				break;
    			}
    		}
    		System.out.println(getName() + " 종료죔");
    	}
    }
    ```
    
- ex) 스레드 그룹을 이용한 일괄 종료 예시
    - ThreadGroupEx.java
    
    ```java
    public class ThreadGroupEx {
    
    	public static void main(String[] args) {
    		// myGroup에 두 스레드를 포함시킴
    		ThreadGroup myGroup = new ThreadGroup("myGroup");
    		WorkThread workThreadA = new WorkThread(myGroup, "workThreadA");
    		WorkThread workThreadB = new WorkThread(myGroup, "workThreadB");
    		
    		workThreadA.start();
    		workThreadB.start();
    		
    		System.out.println("[ main 스레드 그룹의 list() 메소드 출력 내용 ]");
    		ThreadGroup mainGroup = Thread.currentThread().getThreadGroup();
    		mainGroup.list();
    		System.out.println();
    		
    		try { Thread.sleep(1000); } catch(InterruptedException e) {}
    		
    		System.out.println("[ myGroup 스레드 그룹의 interrupt() 메소드 호출 ]");
    		myGroup.interrupt();
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/multi_thread/스레드_그룹의_일괄_interrupt()/Untitled%201.png)
    
    - list() 메소드는 현재 스레드 그룹의 이름과 최대 우선순위를 헤더로 출력
        - 그 아래에 현제 스레드 그룹에 포함된 스레드와 하위 스레드 그룹의 내용을 보여준다.
        - 스레드는 [스레드 이름, 우선순위, 소속 그룹명]으로 출력
    - 스레드 그룹의 `interrupt()` 메소드를 호출하면 myGroup에 포함된 두 스레드에서 InterruptedException이 발생되어 스레드가 종료됨

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판