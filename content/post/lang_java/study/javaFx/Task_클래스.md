---
title: "[Java] JavaFX 스레드 동시성, Task 클래스"
description: ""
date: "2022-12-28T17:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
---
<!--more-->

- `javafx.concurrent` 패키지는 JavaFX 애플리케이션 개발에 사용할 수 있는 스레드 동시성 API를 제공
    - 이 패키지는 Worker 인터페이스와 두 가지의 구현 클래스인 Task, Service로 구성
    
    ![Untitled](/images/lang_java/javaFx/Task_클래스/Untitled.png)
    
    - Worker 인터페이스는 Task와 Service에서 공통적으로 사용할 수 있는 메소드가 선언
    - Task 클래스는 JavaFX 애플리케이션에서 비동이 작업을 표현한 클래스
    - Service는 Task를 간편하게 시작, 취소, 재시작할 수 있는 기능을 제공

## 1. Task 생성

- Task는 작업 스레드에서 실행되는 하나의 작업을 표현한 추상 클래스
- 하나의 작업을 정의할 때에는 Task를 상속해서 클래스를 생성한 후, `call()` 메소드를 재정의하면 된다.
    - `call()` 메소드는 리턴값이 있는 데 리턴값은 작업 결과
- Task는 제네릭 타입
    - 타입 파라미터는 작업 결과 타입
        - 이 타입은 `call()` 메소드의 리턴 타입과 동일해야 한다.
- 작업 결과가 Integer인 Task를 정의
    
    ```java
    Task<Integer> task = new Task<Integer>() {
    	@Override
    	protected Integer call() throws Exception {
    		int result = 0;
    		// 작업 코드...
    		return result;
    	}
    }
    ```
    
- 만약 리턴값이 없는 작업이라면 타입 파라미터로 Void를 사용할 수 있다.
    - 이 경우 `call()` 메소드의 리턴값은 null이다.
    
    ```java
    Task<Void> task = new Task<Void>() {
    	@Override
    	protected Void call() throws Exception {
    		// 작업 코드...
    		return null;
    	}
    }
    ```
    

## 2. Task 실행

- Task를 작업 스레드에서 실행하려면 작업 스레드를 생성할 때 매개값으로 Task를 전달하면 된다.
    - 이것은 Task가 Runnable 인터페이스를 구현하고 있기 때문
- 작업 스레드에서 Task를 처리하는 코드
    
    ```java
    Task<Integer> task = new Task<Integer>() { ... };
    Thread thread = new Thread(task);
    thread.setDaemon(true);
    thread.start();
    ```
    
- Task를 스레드풀의 작업 스레드에서 처리하려면 ExecutorService의 `submit()` 메소드를 호출할 때 매개값으로 전달하면 된다.
    - 스레드풀(ExecutorService)에서 Task를 처리하는 코드
    
    ```java
    Task<Integer> task = new Task<Integer>() { ... };
    executorService.submit(task);
    ```
    

## 3. Task 취소

- Task는 cancel() 메소드가 호출되면 언제든지 실행을 멈출 수 있어야 한다.
- 반목 작업이 포함된 Task는 주기적으로 `isCancelled()` 메소드를 호출해서 작업이 취소되었는지 확인
    - 그 후 작업을 종료한다.
- `cancel()` 메소드가 호출되면 `isCancelled()`는 true를 리턴
    
    ```java
    Task<Void> task = new Task<Void>() {
    	@Override
    	protected Void call() throws Exception {
    		while (...) {
    			if (isCancelled()) { break; }
    			// 작업코드...
    		}
    		return null;
    	}
    };
    ```
    
- 만약 Task가 `Thread.sleep()`과 같이 블로킹되어 있을 때
    - `cancel()` 메소드가 호출되면 InterruptedException이 발생
    - 이 경우, 예외 처리에서 `isCancelled()` 메소드로 `cancel()` 메소드가 호출된 것인지 확인하고 작업을 종료해야 한다.
    
    ```java
    Taxk<Void> task = new Task<Void>() {
    	@Override
    	protected Void call() throws Exception {
    		while ( ... ) {
    			if(isCancelled()) { break; }
    			// 작업 코드...
    			try { Thread.sleep(100); } catch (InterruptedException interrupted) {
    				if(isCancelled()) { break; }
    			}
    		}
    		return null;
    	}
    };
    ```
    
- Task는 한 번만 사용할 수 있다.
    - 재사용할 수 없기 때문에 작업이 완료되었거나 취소된 후에 작업을 재실행하려면 Task를 다시 생성해야 한다.

## 4. UI 변경

- Task의 `call()` 메소드는 작업 스레드상에서 호출되기 때문에 UI 변경 코드를 작성할 수 없다.
    - 작성하게 되면 런타임 예외가 발생
    - 대신 call() 메소드 내부에서는 `updateProgress()`, `updateMessage()` 메소드를 호출할 수 있다.
        - 이 메소드들은 JavaFX Application Thread로 하여금 UI를 업데이트하도록 해준다.
    
    ### (1) updateProgress() 메소드
    
    - 작업 스레드의 진행 정도를 ProgressBar에 나타내는 경우가 종종 있다.
    - Task의 progress 속성과 ProgressBar의 progress 속성이 바인딩되어 있을 경우
        - 작업 스레드에서 `updateProgress()` 메소드를 호출했을 때 JavaFX Application Thread가 ProgressBar의 상태를 변경
        
        ```java
        updateProgress(double workDone, double max);
        ```
        
        - 첫 번째 매개값은 현재 진행 정도
        - 두 번째 매개값은 진행 완료 값
    - 이 메소드는 Task의 Progress, totalWork, workDone 속성을 업데이트
    - Task의 progress 속성과 ProgressBar의 progress 속성을 바인딩해서 `updateProgress()`가 호출될 때마다 ProgressBar의 progress 속성값을 변경하는 코드
        
        ```java
        Task<Void> task = new Task<Void>() {
        	@Override
        	protected Void call() throws Exception {
        		for (int i = 1; i <= 100; i++) {
        			if (isCancelled()) { break; }
        			// 작업 코드...
        		
              // Task의 progress 속성 업데이트
        			updateProgress(i, 100); 		
        		}
            return null;
        	}
        };
        
        // progress 속성 바인딩
        ProgressBar progressBar = new ProgressBar();
        
        // 속성 바인딩
        progressBar.progressProperty()
                      .bind(task.progressProperty());
        
        // 작업 스레드 시작
        Thread thread = new Thread(task);
        thread.start();
        ```
        
    
    ### (2) updateMessage() 메소드
    
    - 작업이 진행 중이거나, 작업이 취소되었을 경우, 문자열 메시지를 전달해서 UI를 변경하고 싶다면 `updateMessage()`를 호출 가능
        
        ```java
        updateMessage(String message);
        ```
        
        - `updateMessage()`는 Task의 message 속성을 업데이트
        - Task의 message 속성과 UI 컨트롤의 문자열 속성을 바인딩할 수 있다.
    - Task의 message 속성과 Label의 text 속성을 바인딩해서 `updateMessage()`가 호출될 때마다 Label의 text 속성값을 변경하는 코드
        
        ```java
        Task<Void> task = new Task<Void>() {
        	@Override
        	protected Void call() throws Exception {
        		for (int i = 1; i <= 100; i++) {
        			if (isCancelled()) { break; }
        
        			// Task의 message 속성 업데이트
        			updateMessage( String.valueOf(i) };
        		}
        		return null;
        	}
        };
        
        // 문자열 속성 바인딩
        Label label = new Label();
        
        // 속성 바인딩
        label.textProperty().bind(task.messageProperty());
        
        // 작업 스레드 시작
        Thread thread = new Thread(task);
        thread.start();
        ```
        
    
    ### (3) Platform.runLater() 메소드
    
    - 단순히 ProgressBar 또는 다른 컨트롤의 문자열 속성에 바인딩하는 것보다 더 복잡한 UI 변경이 필요한 경우
        - `call()` 메소드 내에서 `Platform.runLater(new Runnable() { … })`을 호출하면 된다.
        
        ```java
        Task<Integer> task = new Task<Integer>() {
        	@Override
        	protected Integer call() throws Exception {
        		for(int i = 1; i <= 100; i++) {
        			if (isCancelled()) { break; }
        			Platform.runLater( () -> { // UI 변경코드 } );
        		}
        		return result;
        	}
        };
        
        // 작업 스레드 시작
        Thread thread = new Thread(task);
        thread.start();
        ```
        
- ex) 작업 스레드가 0.1초 주기로 0부터 100까지 카운팅한 값을 ProgressBar의 progress 속성 및 Label의 text 속성과 바인딩해서 UI를 변경
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
                fx:controller="task.RootController">
       <children>
          <ProgressBar fx:id="progressBar" 
                       layoutX="17.0" layoutY="23.0" 
                       prefWidth="200.0" progress="0.0" />
          <Label layoutX="18.0" layoutY="57.0" 
                 text="진행정도" />
          <Label fx:id="lblWorkDone" layoutX="77.0" 
                 layoutY="57.0" />
          <Button fx:id="btnStart" layoutX="66.0" layoutY="91.0" 
                  mnemonicParsing="false" text="시작" />
          <Button fx:id="btnStop" layoutX="130.0" layoutY="91.0"
                  mnemonicParsing="false" text="멈춤" />
       </children>
    </AnchorPane>
    ```
    
    - RootController.java
    
    ```java
    import java.net.URL;
    import java.util.ResourceBundle;
    
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
    	@FXML private Button btnStart;
    	@FXML private Button btnStop;
    	
    	private Task<Void> task;
    	
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    		btnStart.setOnAction(event -> handleBtnStart(event));
    		btnStop.setOnAction(event -> handleBtnStop(event));
    	}
    	
    	public void handleBtnStart(ActionEvent e) {
    		task = new Task<Void>() {
    			@Override
    			protected Void call() throws Exception {
    				for(int i = 0; i <= 100; i++) {
    					if(isCancelled()) {
    						updateMessage("취소됨");
    						break;
    					}
    					// Task의 progress와 message 속성 업데이트
    					updateProgress(i, 100);
    					updateMessage(String.valueOf(i));
    					
    					try { Thread.sleep(100); } catch(InterruptedException e) {
    						if(isCancelled()) {
    							updateMessage("취소됨");
    							break;
    						}
    					}
    				}
    				return null;
    			}
    		};
    		
    		// 속성 바인딩
    		progressBar.progressProperty().bind(task.progressProperty());
    		lblWorkDone.textProperty().bind(task.messageProperty());
    		
    		Thread thread = new Thread(task);
    		thread.setDaemon(true);
    		thread.start();
    	}
    	
    	public void handleBtnStop(ActionEvent e) {
    		task.cancel(); // 작업 취소
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
    
    ![Untitled](/images/lang_java/javaFx/Task_클래스/Untitled%201.png)
    
    ### (4) 작업 상태별 콜백
    
    - 작업이 어떻게 처리됐는지에 따라 Task의 다음 세 가지 메소드 중 하나가 자동 호출(콜백)된다.
        
        
        | 콜백 메소드 | 설명 |
        | --- | --- |
        | succeeded() | 성공적으로 call() 메소드가 리턴되었을 때 |
        | cancelled() | cancel() 메소드로 작업이 취소되었을 때 |
        | failed() | 예외가 발생되었을 때 |
        - 이 메소드들은 Task 클래스를 작성할 때 재정의해서 애플리케이션 로직으로 재구성 가능
        - 주목할 점은 이 메소드들은 작업 스레드상에서 호출되는 것이 아니다.
            - JavaFX Application Thread 상에서 호출되기 때문에 안전하게 UI 변경 코드를 작성 가능
        - 작업 결과가 있는 Task일 경우(`call(`) 메소드가 리턴값이 있을 경우) `succeeded()` 메소드를 재정의해서 작업 결과를 얻을 수 있다.
            - V는 `call()` 메소드가 리턴하는 값의 타입
        
        ```java
        @Override
        protected void succeeded() {
        	V v = task.getValue();
        }
        ```
        
- ex) 0부터 100까지 합을 구하는 Task를 작성하고 작업 스레드에서 실행
    - 작업 완료 결과는 `succeeded()` 메소드에서 얻어 Label 컨트롤에 나타낸다.
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
                fx:controller="task_2.RootController">
       <children>
          <ProgressBar layoutX="17.0" layoutY="23.0" 
                       prefWidth="200.0" progress="0.0" 
                       fx:id="progressBar"/>
          <Label layoutX="18.0" layoutY="57.0" text="진행정도 :"/>
          <Label layoutX="77.0" layoutY="57.0" 
                 fx:id="lblWorkDone" />
          <Label layoutX="118.0" layoutY="57.0" text="작업결과 :" />
          <Label layoutX="175.0" layoutY="57.0" 
                 fx:id="lblResult" />
          <Button layoutX="66.0" layoutY="91.0" 
                  mnemonicParsing="false" text="시작" 
                  fx:id="btnStart" />
          <Button layoutX="130.0" layoutY="91.0" 
                  mnemonicParsing="false" text="멈춤" 
                  fx:id="btnStop" />
       </children>
    </AnchorPane>
    ```
    
    - RootController.java
    
    ```java
    import java.net.URL;
    import java.util.ResourceBundle;
    
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
    	
    	private Task<Integer> task;
    	
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    		btnStart.setOnAction(event -> handleBtnStart(event));
    		btnStop.setOnAction(event -> handleBtnStop(event));
    	}
    	
    	public void handleBtnStart(ActionEvent e) {
    		task = new Task<Integer>() {
    			@Override
    			protected Integer call() throws Exception {
    				int result = 0;
    				for(int i = 0; i <= 100; i++ ) {
    					if(isCancelled()) { break; }
    					result += i;
    					updateProgress(i, 100);
    					updateMessage(String.valueOf(i));
    					try {Thread.sleep(100);} catch(InterruptedException e) {
    						if(isCancelled()) {break;}
    					}
    				}
    				return result;
    			}
    			
    			@Override
    			protected void succeeded() {
    				lblResult.setText(String.valueOf(getValue()));
    			}
    			
    			@Override
    			protected void cancelled() {
    				lblResult.setText("취소됨");
    			}
    			
    			@Override
    			protected void failed() {
    				lblResult.setText("실패");
    			}
    		};
    		
    		progressBar.progressProperty().bind(task.progressProperty());
    		lblWorkDone.textProperty().bind(task.messageProperty());
    		lblResult.setText("");
    		
    		Thread thread = new Thread(task);
    		thread .setDaemon(true);
    		thread.start();
    	}
    	
    	public void handleBtnStop(ActionEvent e) {
    		task.cancel();
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
    
    ![Untitled](/images/lang_java/javaFx/Task_클래스/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판