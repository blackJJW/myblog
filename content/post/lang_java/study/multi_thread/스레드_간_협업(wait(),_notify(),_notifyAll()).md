---
title: "[Java] 스레드 상태 제어, 스레드 간 협업(wait(), notify(), notifyAll())"
description: ""
date: "2022-10-09T23:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 경우에 따라서는 두 개의 스레드를 번갈아가며 실행해야 할 경우가 있다.
    - 정확한 교대 작업이 필요한 경우
        - 자신의 작업이 끝나면 상대방 스레드를 일시 정지 상태에서 풀어준다.
        - 자신은 일시 정지 상태로 만드는 것
        
        ### 이 방법의 핵심은 공유 객체에 있다.
        
        - 공유 객체는 두 스레드가 작업할 내용을 각각 동기화 메소드로 구분
        - 한 스레드가 작업을 완료하면 `notify()` 메소드를 호출해서 일시 정지 상태에 있는 다른 스레드를 실행 대기 상태로 만든다.
        - 자신은 두 번 작업을 하지 않도록 `wait()` 메소드를 호출하여 일시 정지 상태로 만든다.
        
        ![Untitled](/images/lang_java/multi_thread/스레드_간_협업(wait(),_notify(),_notifyAll())/Untitled.png)
        
- 만약 `wait()` 대신 `wait(long timeout)`이나, `wait(long timeout, int nanos)`를 사용하면 `notify()`를 호출하지 않아도 지정된 시간이 지나면 스레드가 자동적으로 실행 대기 상태가 된다.
- `notify()` 메소드와 동일한 역할을 하는 `notifyAll()` 메소드가 존재
    - `notify()`는 wait()에 의해 일시 정지된 스레드 중 한 개를 실행 대기 상태로 만든다.
    - `notifyAll()` 메소드는 `wait()`에 의해 일시 정지된 모든 스레드들을 실행 대기 상태로 만든다.
    - 이 메소드들은 Thread 클래스가 아닌 Object 클래스에 선언된 메소드이므로 모든 공유 객체에서 호출이 가능
    - 주의할 점은 이 메소드들은 동기화 메소드 또는 동기화 블록 내에서만 사용 가능
- ex) 두 스레드의 작업 내용을 동기화 메소드로 작성한 공유 객체
    - 두 스레드의 작업을 WorkObject의 `methodA()`와 `methodB()`에 정의해 두고, 두 스레드 ThreadA와 ThreadB가 교대로 `methodA()`와 `methodB()`를 호출
    - WorkObject.java
    
    ```java
    public class WorkObject {
    	public synchronized void methodA() {
    		System.out.println("ThreadA의 methodA() 작업 실행");
    		notify(); // 일시 정지 상태에 있는 ThreadB를 실행 대기 상태로 만듦
    		try {
    			wait(); // ThreadA를 일시 정지 상태로 만듦
    		} catch(InterruptedException e) {
    		}
    	}
    	
    	public synchronized void methodB() {
    		System.out.println("ThreadB의 methodB() 작업 실행");
    		notify(); // 일시 정지 상태에 있는 ThreadA를 실행 대기 상태로 만듦
    		try {
    			wait(); // ThreadB를 일시 정지 상태로 만듦
    		} catch(InterruptedException e) {
    		}
    	}
    
    }
    ```
    
- ex) WorkObject의 methodA, methodB를 실행하는 메소드
    - ThreadA.java
    
    ```java
    public class ThreadA extends Thread{
    	private WorkObject workObject;
    	
    	public ThreadA(WorkObject workObject) {
    		// 공유 객체를 매개값으로 받아 필드에 저장
    		this.workObject = workObject;
    	}
    	
    	@Override
    	public void run() {
    		// 공유 객체의 methodA를 10번 반복 호출
    		for(int i = 0; i < 10; i++) {
    			workObject.methodA();
    		}
    	}
    }
    ```
    
    - ThreadB.java
    
    ```java
    public class ThreadB extends Thread{
    	private WorkObject workObject;
    	
    	public ThreadB(WorkObject workObject) {
    		// 공유 객체를 매개값으로 받아 필드에 저장
    		this.workObject = workObject;
    	}
    	
    	@Override
    	public void run() {
    		// 공유 객체의 methodB를 10번 반복 호출
    		for(int i = 0; i < 10; i++) {
    			workObject.methodB();
    		}
    	}
    
    }
    ```
    
- ex) 두 스레드를 생성하고 실행하는 메인 스레드
    - WaitNotifyEx.java
    
    ```java
    public class WaitNotifyEx {
    
    	public static void main(String[] args) {
    		// 공유 객체 생성
    		WorkObject sharedObject = new WorkObject();
    		
    		// ThreadA와 ThreadB 생성
    		ThreadA threadA = new ThreadA(sharedObject);
    		ThreadB threadB = new ThreadB(sharedObject);
    		
    		// ThreadA와 ThreadB 실행
    		threadA.start();
    		threadB.start();
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/multi_thread/스레드_간_협업(wait(),_notify(),_notifyAll())/Untitled%201.png)
    
- ex) 두 스레드의 작업 내용을 동기화 메소드로 작성한 공유 객체
    - 데이터를 저장하는 스레드(생산자 스레드)가 데이터를 저장하면, 데이터를 소비하는 스레드(소비자 스레드)가 데이터를 읽고 처리하는 교대 작업을 구현
    
    ![Untitled](/images/lang_java/multi_thread/스레드_간_협업(wait(),_notify(),_notifyAll())/Untitled%202.png)
    
    - 생산자 스레드는 소비자 스레드가 읽기 전에 새로운 데이터를 두 번 생성하면 안된다.(`setData()` 메소드를 두 번 실행하면 안 됨)
    - 소비자 스레드는 생산자 스레드가 새로운 데이터를 생성하기 전에 이전 데이터를 두 번 읽어서는 안된다.(`getData()` 메소드를 두 번 실행하면 안 됨)
    - 구현 방법
        - 공유 객체(DataBox)에 데이터를 저장할 수 있는 data 필드의 값이 null이면 생산자 스레드를 실행 대기 상태로 만든다.
        - 소비자 스레드를 일시 정지 상태로 만든다.
        - 반대로, data 필드의 값이 null이 아니면 소비자 스레드를 실행 대기 상태로 만든다.
            - 생산자 스레드를 일시 정지 상태로 만든다.
    - DataBox.java
    
    ```java
    public class DataBox {
    	private String data;
    	
    	public synchronized String getData() {
    		// data 필드가 null이면 소비자 스레드를 일시 정지 상태로 만듦
    		if(this.data == null) {
    			try {
    				wait();
    			}catch(InterruptedException e) {}
    		}
    		String returnValue = data;
    		System.out.println("ConsumerThread가 읽은 데이터 : " + returnValue);
    		
    		// data 필드를 null로 만들고 생산자 스레드를 실행 대기 상태로 만듦
    		data = null;
    		notify();
    		
    		return returnValue;
    	}
    	
    	public synchronized void setData(String data) {
    		// data 필드가 null이면 소비자 스레드를 일시 정지 상태로 만듦
    		if(this.data != null) {
    			try {
    				wait();
    			}catch(InterruptedException e) {}
    		}
    		
    		// data 필드에 값을 저장하고 소비자 스레드를 실행 대기 상태로 만듦
    		this.data = data;
    		System.out.println("ProducerThread가 생성한 데이터 : " + data);
    		notify();
    	}
    
    }
    ```
    
- ex) 데이터를 생산(저장)하는 스레드
    - ProducerThread.java
    
    ```java
    public class ProducerThread extends Thread{
    	private DataBox dataBox;
    	
    	public ProducerThread(DataBox dataBox) {
    		// 공육 객체를 필드에 저장
    		this.dataBox = dataBox;
    	}
    	
    	@Override
    	public void run() {
    		for(int i = 0; i <= 3; i++) {
    			String data = "Data-" + i;
    			// 새로운 데이터를 저장
    			dataBox.setData(data);
    		}
    	}
    
    }
    ```
    
- ex) 데이터를 소비하는(읽는) 스레드
    - ConsumerThread.java
    
    ```java
    public class ConsumerThread extends Thread{
    	private DataBox dataBox;
    	
    	public ConsumerThread(DataBox dataBox) {
    		// 공유 객체를 필드에 저장
    		this.dataBox = dataBox;
    	}
    	
    	@Override
    	public void run() {
    		for(int i = 1; i <= 3; i++) {
    			// 새로운 데이터를 읽음
    			String data = dataBox.getData();
    		}
    	}
    
    }
    ```
    
- ex) 두 스레드를 생성하고 실행하는 메인 스레드
    - WaitNotifyEx.java
    
    ```java
    public class WaitNotifyExample {
    
    	public static void main(String[] args) {
    		DataBox dataBox = new DataBox();
    		
    		ProducerThread producerThread = new ProducerThread(dataBox);
    		ConsumerThread consumerThread = new ConsumerThread(dataBox);
    		
    		producerThread.start();
    		consumerThread.start();
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/multi_thread/스레드_간_협업(wait(),_notify(),_notifyAll())/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판