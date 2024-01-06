---
title: "[Java] JavaFX, 이벤트 핸들러(EventHandler)"
description: ""
date: "2022-12-16T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- JavaFX는 이벤트 발생 컨트롤과 이벤트 핸들러(EventHandler)를 분리하는 위임형(Delegation) 방식을 사용
    - 위임형 방식 : 컨트롤에서 이벤트가 발생하면, 컨트롤이 직접 처리하지 않고 이벤트 핸들러에게 이벤트 처리를 위임하는 방tlr
        - ex) 사용자가 Button을 클릭하면 ActionEvent가 발생
            - Button에 등록된 EventHandler가 ActionEvent를 처리
    
    ![Untitled](/images/lang_java/javaFx/이벤트_핸들러(EventHandler)/Untitled.png)
    
    - EventHandler는 컨트롤에서 이벤트가 발생하면, 자신의 `handle()` 메소드를 실행
        - `handle()` 메소드에는 윈도우 닫기, 컨트롤 내용 변경, 다이얼로그 띄우기 등의 다양한 코드를 작성 가능
        - EventHandler는 제네릭 타입이기 때문에 타입 파라미터는 발생된 이벤트의 타입이 된다.
            - ex) ActionEvent를 처리하는 핸들러는 `EventHandler<ActionEvent>`가 된다.
                - MouseEvent를 처리하는 핸들러는 `EventHandler<MouseEvent>`가 된다.
- EventHadler가 컨트롤에서 발생된 이벤트를 처리하려면 먼저 컨트롤에 EventHandler를 등록해야 한다.
    - 컨트롤은 발생되는 이벤트에 따라 EventHandler를 등록하는 다양한 메소드가 존재
        - 이 메소드들은 `setOnXXX()` 이름을 가지고 있다.
            - XXX는 보통 이벤트 이름과 동일
    - ex) Button을 클릭할 때 발생하는 ActionEvent를 처리하는 `EventHandler<ActionEvent>`를 등록하려면 `setOnAction()` 메소드를 사용
        
        ```java
        Button button = new Button();
        button.setOnAction(new EventHandler<ActionEvent>() {
        	@Override
        	public void handle(ActionEvent event) { ... }
        });
        ```
        
    - ex) TableView의 행을 클릭할 때 발생하는 MouseEvent를 처리하는 `EventHandler<MouseEvent>`를 등록하려면 `setOnMouseClicked()` 메소드를 사용
        
        ```java
        TableView tableView = new TableView();
        tableView.setOnMouseClicked(new EventHandler(){
        	@Override
        	public void handle(MouseEvent event) { ... }
        });
        ```
        
    - ex) 윈도우(Stage)의 우측 상단 닫기(x) 버튼을 클릭했을 때 발생하는 WindowEvent를 처리하는 `EventHandler<WindowEvent>`를 등록하면 `setOnCloseRequest()` 메소드를 사용
        
        ```java
        stage.setOnCloseRequest(new EventHandler<WindowEvent>() {
        	@Override
        	public void handle(WindowEvent event) { ... }
        });
        ```
        
- EventHandler는 하나의 메소드를 가진 함수적 인터페이스이므로 람다식을 이용하면 보다 적은 코드로 EventHandler를 등록할 수 있다.
    
    ```java
    button.setOnAction( event -> { ... } );
    tableView.setMouseClicked( event -> { ... } );
    stage.setOnCloseRequest( event -> { ... } );
    ```
    
- ex) EventHandler를 이용한 이벤트 처리
    - 프로그램적 레이아웃을 작성
    - 버튼의 ActionEvent를 처리
    - 첫 번째 버튼은 직접 EventHandler 객체를 생성한 후 등록
    - 두 번째 버튼은 람다식을 이용해서 EventHandler를 등록
    - AppMain.java
    
    ```java
    import javafx.application.Application;
    import javafx.event.ActionEvent;
    import javafx.event.EventHandler;
    import javafx.geometry.Pos;
    import javafx.scene.Scene;
    import javafx.scene.control.Button;
    import javafx.scene.layout.HBox;
    import javafx.stage.Stage;
    
    public class AppMain extends Application {
    	@Override
    	public void start(Stage primaryStage) throws Exception {
    		HBox root = new HBox();
    		root.setPrefSize(200, 50);
    		root.setAlignment(Pos.CENTER); // 수평 중앙 정렬
    		root.setSpacing(20);
    		
    		Button btn1 = new Button("버튼 1");
    		btn1.setOnAction(new EventHandler<ActionEvent>() {
    			@Override
    			public void handle(ActionEvent event) {
    				System.out.println("버튼 1 클릭");
    			}
    		});
    		
    		Button btn2 = new Button("버튼 2");
    		btn2.setOnAction( event -> System.out.println("버튼 2 클릭") );
    		
    		root.getChildren().addAll(btn1, btn2); // HBox에 btn1, btn2 추가
    		Scene scene = new Scene(root);
    		
    		primaryStage.setTitle("AppMain");
    		primaryStage.setScene(scene);
    		primaryStage.setOnCloseRequest( event -> System.out.println("종료 클릭") );
    		primaryStage.show();
    	}
    	public static void main(String[] args) {
    		launch(args);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/이벤트_핸들러(EventHandler)/Untitled%201.png)
    
    ![Untitled](/images/lang_java/javaFx/이벤트_핸들러(EventHandler)/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판