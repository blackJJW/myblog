---
title: "[Java] JavaFX 스레드 동시성, Platform.runLater() 메소드"
description: ""
date: "2022-12-28T16:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
---
<!--more-->

- 작업 스레드가 직접 UI를 변경할 수 없기 때문에 UI 변경이 필요한 경우
    - 작업 스레드는 UI 변경 코드를 Runnable로 생성
    - 이것을 매개값으로 해서 Platform의 정적 메소드인 `runLater()`를 호출할 수 있다.
    
    ![Untitled](/images/lang_java/javaFx/Platform_runLater()_메소드/Untitled.png)
    
- `runLater()` 메소드는 이벤트 큐(event queue)에 Runnable을 저장하고 즉시 리턴
    - 이벤트 큐에 저장된 Runnable들은 저장된 순서에 따라서 JavaFX Application Thread에 의해 하나씩 실행 처리되어 UI 변경 작업을 한다.
- `runLater()`는 주어진 매개값 Runnable이 바로 처리되는 것이 아니라 이벤트 큐에 먼저 저장된 Runnable을 처리한 후에 처리
    
    ![Untitled](/images/lang_java/javaFx/Platform_runLater()_메소드/Untitled%201.png)
    
- ex) 작업 스레드가 0.1초 주기로 얻어낸 시간을 이용해서 JavaFX Application Thread가 Label 컨트롤의 text 속성을 변경
    - Label은 UI 요소이므로 작업 스레드에서 `setText()` 메소드로 text 속성을 변경할 수 없다.
    - 대신 Runnable을 생성해서 `Platform.runLater()` 메소드를 호출
    - JavaFX Application Thread는 이벤트 큐에서 Runnable을 얻어 안전하게 Label의 `text()` 속성을 변경
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.layout.AnchorPane?>
    
    <AnchorPane prefHeight="100.0" prefWidth="200.0" 
                xmlns:fx="http://javafx.com/fxml/1" 
                xmlns="http://javafx.com/javafx/19"
                fx:controller="runLater.RootContoller">
       <children>
          <Label fx:id="lblTime" alignment="CENTER" 
                 layoutX="25.0" layoutY="15.0" prefHeight="35.0" 
                 prefWidth="150.0" 
                 style="-fx-background-color: black; 
                        -fx-text-fill: yellow; 
                        -fx-font-size: 20; 
                        -fx-background-radius: 10;" 
                text="00:00:00" />
          <Button fx:id="btnStart" layoutX="46.0" layoutY="63.0" mnemonicParsing="false" text="시작" />
          <Button fx:id="btnStop" layoutX="110.0" layoutY="63.0" mnemonicParsing="false" text="멈춤" />
       </children>
    </AnchorPane>
    ```
    
    - RootController.java
    
    ```java
    import java.net.URL;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    import java.util.ResourceBundle;
    
    import javafx.application.Platform;
    import javafx.event.ActionEvent;
    import javafx.fxml.FXML;
    import javafx.fxml.Initializable;
    import javafx.scene.control.Button;
    import javafx.scene.control.Label;
    
    public class RootContoller implements Initializable {
    	@FXML private Label lblTime;
    	@FXML private Button btnStart;
    	@FXML private Button btnStop;
    	
    	private boolean stop;
    	
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    		btnStart.setOnAction(event -> handleBtnStart(event));
    		btnStop.setOnAction(event -> handleBtnStop(event));
    	}
    	
    	public void handleBtnStart(ActionEvent e) {
    		stop = false;
    		Thread thread = new Thread() {
    			@Override
    			public void run() {
    				SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
    				while(!stop) {
    					String strTime = sdf.format(new Date());
    					
    					// UI 변경 작업
    					Platform.runLater(() -> {
    						lblTime.setText(strTime);
    					});
    					try { Thread.sleep(100); } catch (InterruptedException e) {}
    				}
    			};
    		};
    		thread.setDaemon(true);
    		thread.start();
    	}
    	
    	public void handleBtnStop(ActionEvent e) {
    		stop = true;
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
    		Parent root = (Parent)FXMLLoader.load(getClass().getResource("Root.fxml"));
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
    
    ![Untitled](/images/lang_java/javaFx/Platform_runLater()_메소드/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판