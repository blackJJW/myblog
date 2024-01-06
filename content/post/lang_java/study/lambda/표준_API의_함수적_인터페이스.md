---
title: "[Java] 람다식, 표준 API의 함수적 인터페이스"
description: ""
date: "2022-10-30T15:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 자바에서 제공되는 표준 API에서 한 개의 추상 메소드를 가지는 인터페이스들은 모두 람다식을 이용해서 익명 구현 객체로 표현이 가능
    - ex) 스레드의 작업을 정의하는 Runnable 인터페이스는 매개 변수와 리턴값이 없는 run() 메소드만 존재하기 때문에
        - 다음과 같이 람다식을 이용해서 Runnable 인스턴스를 생성 가능
        - RunnableEx.java
    
    ```java
    public class RunnableEx {
    
    	public static void main(String[] args) {
    		// 람다식(스레드가 실행되는 코드)
    		Runnable runnable = () -> {
    			for(int i = 0; i < 10; i++) {
    				System.out.println(i);
    			}
    		};
    		
    		Thread thread = new Thread(runnable);
    		thread.start();
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/표준_API의_함수적_인터페이스/Untitled.png)
    
    - Thread 생성자를 호출할 때 다음과 같이 람다식을 매개값으로 대입해도 된다.
        
        ```java
        Thread thread = new Thread(() -> {
        	for(int i = 0; i < 10; i++){
        		System.out.println(i);
        	}
        });
        ```
        
- 자바 8부터는 빈번하게 사용되는 함수적 인터페이스 (functional interface)는 `java.util.function` 표준 API 패키지로 제공
    - 이 패키지에서 제공하는 함수적 인터페이스의 목적은 메소드 또는 생성자의 매개 타입으로 사용되어 람다식을 대입할 수 있도록 하기 위해
    - 자바 8부터 추가되거나 변경된 API에서 이 함수적 인터페이스들을 매개 타입으로 많이 사용
        - 개발하는 메소드에도 이 함수적 인터페이스들을 매개 타입으로 사용 가능
- `java.util.function` 패키지의 함수적 인터페이스는 크게 Consumer, Supplier, Function, Operator, Predicate로 구분
    - 구분 기준은 인터페이스에 선언된 추상 메소드의 매개값과 리턴값의 유무
    
    ![Untitled](/images/lang_java/lambda/표준_API의_함수적_인터페이스/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판