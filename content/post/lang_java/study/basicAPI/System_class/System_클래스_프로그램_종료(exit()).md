---
title: "[Java] System 클래스, 프로그램 종료(exit())"
description: ""
date: "2022-09-21T20:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 자바 프로그래믄 OS에서 바로 실행되는 것이 아니라 JVM 위에서 실행
    - OS의 모든 기능을 자바 코드로 직접 접근하는 것은 어렵다.
    - `java.lang` 패키지에 속하는 System 클래스를 이용하면 OS의 일부 기능을 이용 가능
        - 프로그램 종료, 키보드로부터 입력, 모니터로 출력, 메모리 정리, 현재 시간 읽기, 시스템 프로퍼티 읽기, 환경 변수 읽기 등이 가능
- System 클래스의 모든 필드와 메소드는 정적(static) 필드와 정적(static) 메소드로 구성

---

# 프로그램 종료(`exit()`)

- 경우에 따라서는 강제적으로 JVM을 종료시킬 때가 존재
    - 이때 System 클래스의 `exit()` 메소드를 호출하면 된다.
    - 현재 실행하고 있는 프로세스를 강제 종료시키는 역할
- `exit()` 메소드는 int 매개값을 지정하도록 되어있다.
    - 이 값을 종료 상태값이라고 한다.
    - 일반적으로 정상 종료일 경우 0으로 지정하고 비정상 종료일 경우 0이외의 다른 값을 준다.
    
    ```java
    System.exit(0);
    ```
    
    - 어떤 값을 주더라도 종료가 가능
    - 특정 값이 입력되었을 경우에만 종료되고 싶다면 자바의 보안 관리자를 직접 설정해서 종료 상태값을 확인하면 된다.
- `System.exit()`가 실행되면 보안 관리자의 `checkExit()` 메소드가 자동 호출
    - 이 메소드에서 종료 상태값을 조사해서 특정 값이 입력되지 않으면 SecurityException을 발생시켜 `System.exit()`을 호출한 곳에서 예외 처리를 할 수 있도록 해준다.
    - `checkExit()`가 정상적으로 실행되면 JVM은 종료
- ex) 종료 상태값으로 5가 입력되면 JVM을 종료하도록 보안 관리자를 설정
    
    ```java
    System.setSecurityManager(new SecurityManager(){
    	@Override
    	public void checkExit(int status){
    		if(status != 5){
    			throw new SecurityException();
    		}
    	}
    });
    ```
    
- ex) exit 메소드
    - 종료 상태값이 5일 경우에만 프로세스를 종료
    - ExitEx.java
    
    ```java
    public class ExitEx {
    
    	public static void main(String[] args) {
    		// 보안 관리자 설정
    		System.setSecurityManager(new SecurityManager() {
    			@Override
    			public void checkExit(int status) {
    				if(status != 5) {
    					throw new SecurityException();
    				}
    			}
    		});
    		
    		for(int i = 0; i < 10; i++) {
    			// i값 출력
    			System.out.println(i);
    			try {
    				// JVM 종료 요청
    				System.exit(i);
    			} catch(SecurityException e) {}
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/System_클래스_프로그램_종료(exit())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판