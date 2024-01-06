---
title: "[Java] JavaFX 라이프사이클"
description: ""
date: "2022-12-12T22:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- JavaFX 애플리케이션은 `Application.launch()` 메소드부터 시작해서 다음과 같은 순서로 진행
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_라이프사이클/Untitled.png)
    
- 메인 클래스의 기본 생성자를 호출해서 객체를 생성
    - 이어서 `init()` 메소드를 호출
        - `init()` 메소드는 메인 클래스의 실행 매개값을 얻어 애플리케이션이 이용할 수 있도록 해준다
    - `init()`이 끝나고 나면, `start()` 메소드를 호출해서 메인 윈도우를 실행시킨다.
- JavaFX 애플리케이션이 종료되는 경우는 다음과 같이 세 가지가 있다.
    1. 마우스로 마지막 윈도우(Stage)의 우측 상단 닫기 버튼을 클릭했을 경우
    2. 자바 코드로 마지막 윈도우(Stage)의 `close()` 메소드를 호출했을 경우
    3. 자바 코드로 `Platform.exit()` 또는 `System.exit(0)`을 호출했을 경우
- JavaFX 애플리케이션은 종료되기 직전에 `stop()` 메소드를 호출
    - `stop()` 메소드는 애플리케이셔니 종료되기 전에 자원을 정리(파일 닫기, 네트워크 끊기)할 기회를 준다.
    - `init()`과 `stop()` 메소드는 옵션으로 필요할 경우 재정의해서 사용하면 된다.
- 주목할 점
    - 라이프사이클의 각 단계에서 호출되는 메소드는 서로 다른 스레드상에서 실행된다는 것이다.
    - JVM이 main 스레드가 `Application.launch()`를 실행하면 `launch()` 메소드는 다음과 같은 이름을 가진 두 가지 스레드를 생성시키고 시작시킨다.
        - **JavaFX-Launcher** : `init()` 실행
        - **JavaFX Application Thread** : 메인 클래스 기본 생성자, `start()` 및 `stop()` 실
    - JavaFX 애플리케이션에서 윈도우(Stage)를 비롯한 UI 생성 및 수정 작업, 입력 이벤트 처리 등은 모두 **Java Application Thread**가 관장
        - JavaFX API는 스레드에 안전하지 않아 멀티 스레드가 동시에 UI를 생성하거나 수정하게 되면 문제가 생기기 때문
        - JavaFX Application Thread만 UI를 생성하거나 수정할 수 있다.
            - 다른 스레드가 UI에 접근하게 되면 예외가 발생
    - `init()` 메소드에서 UI를 생성하는 코드를 작성하면 안된다.
        - `init()` 메소드가 JavaFX-Launcher 스레드에서 실행되기 때문
- ex) 라이프사이클
    - 기본 생성자, `init()`, `start()`, `stop()` 메소드가 어떤 스레드상에서 실행되는지 보여준다.
    
    ```java
    import javafx.application.Application;
    import javafx.stage.Stage;
    
    public class Main extends Application {
    	public Main() {
    		System.out.println(Thread.currentThread().getName() + 
    				" : Main() 호출");
    	}
    	
    	@Override
    	public void init() throws Exception{
    		System.out.println(Thread.currentThread().getName() + 
    				" : init() 호출");
    	}
    	
    	@Override
    	public void start(Stage primaryStage) throws Exception {
    		System.out.println(Thread.currentThread().getName() + 
    				" : start() 호출");
    		primaryStage.show(); // 윈도우 보여주기
    	}
    	
    	@Override
    	public void stop() throws Exception {
    		System.out.println(Thread.currentThread().getName() + 
    				" : stop() 호출");
    	}
    	
    	public static void main(String[] args) {
    		System.out.println(Thread.currentThread().getName() + 
    				" : main() 호출");
    		launch(args); // Main 객체 생성 및 메인 윈도우 생성
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_라이프사이클/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판