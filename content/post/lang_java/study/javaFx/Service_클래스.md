---
title: "[Java] JavaFX 스레드 동시성, Service 클래스"
description: ""
date: "2022-12-29T12:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
---
<!--more-->

- 작업 스레드 상에서 Task를 간편하게 시작, 취소, 재시작할 수 있는 기능을 제공
- Service 클래스의 목적은 작업 스레드와 JavaFX Application Thread가 올바르게 상호작용을 할 수 있도록 도와주는 것

## 1. Service 생성

- Service는 추상 클래스이므로 새로운 Service를 생성하려면 Service를 상속받고 `createTask()` 메소드를 재정의해야 한다.
    - `createTask()`는 작업 스레드가 실행할 Task를 생성해서 리턴하도록 해야 한다.
- Service는 제네릭 타입이기 때문에 타입 파라미터는 작업 결과 타입으로 지정
- 작업 결과가 Integer인 Service를 정의
    
    ```java
    class CustomService extends Service<Integer> {
    	@Override
    	protected Task<Integer> createTask() {
    		Task<Integer> task = new Task<Integer>() { ... };
    		return task;
    	}
    }
    ```
    
- 작업 결과가 없는 Service 클래스를 정의
    
    ```java
    class CustomService extends Service<Void> {
    	@Override
    	protected Task<Void> createTask() {
    		Task<Void> task = new Task<Void>() { ... };
    		return task;
    	}
    }
    ```
    

## 2. Service 시작, 취소, 재시작

- Service를 시작하려면 `start()` 메소드를 호출
    - 취소하려면 `cancel()` 메소드를 호출
    - 재시작이 필요한 경우 `restart()`를 호출
    - 주의할 점은 이 메소드들은 JavaFX Application Thread 상에서 호출해야 한다.
- `start()`와 `restart()`가 호출되면 매번 `createTask()`가 실행
    - 리턴된 Task를 받아 작업 스레드에서 실행
- ex) JavaFX 애플리케이션이 시작하면 TimeService를 시작
    - btnStop을 클릭하면 취소
    - btnStart를 클릭하면 재시작되도록 컨트롤러를 구성
    
    ```java
    public class RootController implements Initializable {
    	private TimeService timeService;
    
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    		timeService = new TimeService(); // TimeService 객체 생성
    		timeService.start();
    	}
    
    	public void handleBtnStart(ActionEvent e) {
    		timeService.restart();
    	}
    
    	public void handleBtnStop(ActionEvent e) {
    		timeService.cancel();
    	}
    
    	// TimeService 클래스 선언
    	class TimeService extends Service<Void> {
    		@Override
    		protected Task<Void> createTask() {
    			Task<Void> task = new Task<Void>() { ... };
    			return task;
    		}
    	}
    }
    ```
    

## 3. 작업 상태별 콜백

- Task와 마찬가지로 Service도 작업이 어떻게 처리됐느냐에 따라서 Service의 다음 세 가지 메소드 중 하나가 자동 호출(콜백)된다.
    
    
    | 콜백 메소드 | 설명 |
    | --- | --- |
    | succeeded() | 성공적으로 call() 메소드가 리턴되었을 때 |
    | cancelled() | cancel() 메소드로 작업이 취소되었을 때 |
    | failed() | 예외가 발생되었을 때 |
    - 이 메소드들은 Service 클래스를 작성할 때 재정의해서 애플리케이션 로직으로 재구성할 수 있다.
    - Task와 마찬가지로 이 메소드들은 작업 스레드상에서 호출되는 것이 아니라, JavaFX Application Thread상에서 호출되기 때문에 안전하게 UI 변경 코드 작성할 수 있다.
    - 작업 결과가 있는 Service일 경우 `succeeded()` 메소드에서 작업 결과를 다음과 같이 얻을 수 있다.
        - V는 작업 결과 타입
    
    ```java
    @Override
    protected void succeeded() {
    	V v = service.getValue();
    }
    ```
    
- ex) Service 이용
    - 0부터 100까지 합을 구하는 Service를 작성
    - 작업 스레드에서 실행
    - 작업 완료 결과는 `succeeded()` 메소드에서 얻어 Label 컨트롤에 나타난다.
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.control.ProgressBar?>
    <?import javafx.scene.layout.AnchorPane?>
    
    <AnchorPane prefHeight="129.0" prefWidth="233.0" 
                xmlns:fx="http://javafx.com/fxml/1" 
                xmlns="http://javafx.com/javafx/19"
                fx:controller="service.RootController">
       <children>
          <ProgressBar layoutX="17.0" layoutY="23.0" 
                       prefWidth="200.0" progress="0.0" 
                       fx:id="progressBar"/>
          <Label layoutX="18.0" layoutY="57.0" text="진행정도 :" />
          <Label layoutX="77.0" layoutY="57.0" 
                 fx:id="lblWorkDone" />
          <Label layoutX="118.0" layoutY="57.0" text="작업결과 :" />
          <Label layoutX="175.0" layoutY="57.0" 
                 fx:id="lblResult"/>
          <Button layoutX="66.0" layoutY="91.0" 
                  mnemonicParsing="false" text="시작" 
                  fx:id="btnStart"/>
          <Button layoutX="130.0" layoutY="91.0" 
                  mnemonicParsing="false" text="멈춤" 
                  fx:id="btnStop"/>
       </children>
    </AnchorPane>
    ```
    
    - RootController.java
    
    ```java
    import java.net.URL;
    import java.util.ResourceBundle;
    
    import javafx.concurrent.Service;
    import javafx.concurrent.Task;
    import javafx.event.ActionEvent;
    import javafx.fxml.FXML;
    import javafx.fxml.Initializable;
    import javafx.scene.control.Button;
    import javafx.scene.control.Label;
    import javafx.scene.control.ProgressBar;
    
    public class RootController implements Initializable {
    	@FXML private ProgressBar progressBar;
    	@FXML private Label lblWorkDone;
    	@FXML private Label lblResult;
    	@FXML private Button btnStart;
    	@FXML private Button btnStop;
    	
    	private TimeService timeService;
    	
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    		btnStart.setOnAction( event -> handleBtnStart(event) );
    		btnStop.setOnAction( event -> handleBtnStop(event) );
    		
    		timeService = new TimeService();
    		timeService.start();
    	}
    	
    	public void handleBtnStart(ActionEvent e) {
    		timeService.restart();
    		lblResult.setText("");
    	}
    	
    	public void handleBtnStop(ActionEvent e) {
    		timeService.cancel();
    	}
    	
    	class TimeService extends Service<Integer>{
    		@Override
    		protected Task<Integer> createTask(){
    			Task<Integer> task = new Task<Integer>() {
    				@Override
    				protected Integer call() throws Exception {
    					int result = 0;
    					for(int i = 0; i <= 100; i++) {
    						if(isCancelled()) { break; }
    						result += i;
    						updateProgress(i, 100);
    						updateMessage(String.valueOf(i));
    						try { Thread.sleep(100); } catch(InterruptedException e) {
    							if(isCancelled()) { break; }
    						}
    					}
    					return result;
    				}
    			};
    			progressBar.progressProperty().bind(task.progressProperty());
    			lblWorkDone.textProperty().bind(task.messageProperty());;
    			return task;
    		}
    		
    		// 성공적으로 작업이 완료되었을 때 호출
    		@Override
    		protected void succeeded() {
    			lblResult.setText(String.valueOf(getValue()));
    		}
    		
    		// 작업이 취소되었을 때 호출
    		@Override
    		protected void cancelled() {
    			lblResult.setText("취소됨");
    		}
    		
    		// 작업 처리 도중 예외가 발생할 경우 호출
    		@Override
    		protected void failed() {
    			lblResult.setText("실패");
    		}
    	}
    }
    ```
    
    - AppMain.java
    
    ```java
    import javafx.application.Application;
    import javafx.fxml.FXMLLoader;
    import javafx.scene.Parent;
    import javafx.scene.Scene;
    import javafx.stage.Stage;
    
    public class AppMain extends Application {
    	@Override
    	public void start(Stage primaryStage) throws Exception {
    		Parent root = (Parent) FXMLLoader.load(getClass().getResource("Root.fxml"));
    		Scene scene = new Scene(root);
    		
    		primaryStage.setTitle("AppMain");
    		primaryStage.setScene(scene);
    		primaryStage.show();
    	}
    
    	public static void main(String[] args) {
    		launch(args);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/Service_클래스/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판