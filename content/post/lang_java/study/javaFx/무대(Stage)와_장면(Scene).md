---
title: "[Java] JavaFX, 무대(Stage)와 장면(Scene)"
description: ""
date: "2022-12-13T01:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- JavaFX는 윈도우를 무대(Stage)로 표현
    - 무대는 한 번에 하나의 장면을 가질 수 있다.
    - JavaFX는 장면을 `javafx.scene.Scene`으로 표현
- 메인 윈도우는 `start()` 메소드의 primaryStage 매개값으로 전달
    - 장면(Scene)은 직접 생성해야 한다.
- Scene을 생성하려면 UI의 루트 컨테이너인 `javafx.scene.Parent`가 필요
    
    ```java
    Scene scene = new Scene(Parent root);
    ```
    
    - Parent는 추상 클래스이기 때문에 하위 클래스로 객체를 생성해서 제공해야 한다.
    - 주로 `javafx.scene.layout` 패키지의 컨테이너들이 사용된다.
        
        ![Untitled](/images/lang_java/javaFx/무대(Stage)와_장면(Scene)/Untitled.png)
        
    - 실제 UI 컨트롤들이 추가되는 곳은 Parent가 되고, Parent의 폭과 높이가 장면의 폭과 높이가 된다.
- 장면(Scene)을 생성한 후에는 윈도우(Stage)에 올려야 하는데, Stage의 `setScene()` 메소드를 사용
    - `setScene()` 메소드는 매개값으로 받은 Scene을 윈도우 내용으로 설정
        
        ```java
        primaryStage.setScene(scene);
        ```
        
- ex) Stage와 Scene
    - Parent의 하위 클래스인 `javafx.scene.layout.VBox`를 이용해서 Scene을 생성
    - 메인 윈도우(primaryStage)의 장면으로 설정
    - VBox에는 Label과 Button 컨트롤을 배치
    - Main.java
    
    ```java
    import javafx.application.Application;
    import javafx.application.Platform;
    import javafx.geometry.Pos;
    import javafx.scene.Scene;
    import javafx.scene.control.Button;
    import javafx.scene.control.Label;
    import javafx.scene.layout.VBox;
    import javafx.scene.text.Font;
    import javafx.stage.Stage;
    
    public class Main extends Application {
    	@Override
    	public void start(Stage primaryStage) throws Exception {
    		VBox root = new VBox();         // Parent 하위 객체인 VBox 생성
    		root.setPrefWidth(350);         // VBox의 폭 설정
    		root.setPrefHeight(150);        // VBox의 높이 설정
    		root.setAlignment(Pos.CENTER);  // 컨트롤을 중앙으로 정렬
    		root.setSpacing(20);            // 컨트롤의 수직 간격
    		
    		Label label = new Label();      // Label 컨트롤 생성
    		label.setText("Hello, JavaFX"); // text 속성 설정
    		label.setFont(new Font(50));    // font 속성 설정
    		
    		Button button = new Button();   // Button 컨트롤 생성
    		button.setText("확인");          // text 속성 설정
    		button.setOnAction(event -> Platform.exit());
    		                                // 클릭 이벤트 처리
    		
    		root.getChildren().add(label);  // VBox의 자식으로 Label 컨트롤 추가
    		root.getChildren().add(button); // VBox의 자식으로 Button 컨트롤 추가
    		
    		Scene scene = new Scene(root);  // VBox를 루트 컨테이너로 해서 Scene 생성
    		
    		primaryStage.setTitle("Main");  // 윈도우의 제목 설정
    		primaryStage.setScene(scene);   // 윈도우에 장면 설정
    		primaryStage.show();            // 윈도우 보여주기
    	}
    	
    	public static void main(String[] args) {
    		launch(args); 
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/무대(Stage)와_장면(Scene)/Untitled%201.png)
    
    - VBox는 수직 방향으로 자식 컨트롤을 배치하는 컨테이너로 먼저 Label을 배치
        - 그 아래에 Button을 배치
    - `button.setOnAction(event -> Platform.exit());` 는 Button을 클릭했을 때 발생하는 ActionEvent를 람다식으로 처리한 것
        - Button을 클릭하면 `Platform.exit()`를 호출해서 애플리케이션을 종료

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판