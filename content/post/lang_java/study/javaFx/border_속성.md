---
title: "[Java] JavaFX CSS 스타일, border 속성"
description: ""
date: "2022-12-27T11:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
  - "CSS"
---
<!--more-->

- border 속성은 컨테이너 및 컨트롤의 경계선 스타일을 설정
- 경계선의 굵기, 색상, 스타일, 내부 경계선의 위치를 설정할 수 있는 세부 속성을 가지고 있다.
    
    
    | 속성 | 설명 |
    | --- | --- |
    | -fx-border-color | 경계선의 색상 |
    | -fx-border-insets | 내부 경계선의 위치 |
    | -fx-border-radius | 둥근 모서리를 위한 원의 반지름 |
    | -fx-border-style | 경계선의 스타일(실선, 점선) |
    | -fx-border-width | 경계선의 굵기 |
- -fx-border-color 속성은 경계선의 색상을 설정하는 데 다양한 값들을 사용 가능
    
    ```css
    -fx-border-color: red;     // 색이름
    -fx-border-color: #ff0000; // #색상번호
    -fx-border-color: rgba(255, 0, 0, 0); 
                   // rgba(red값, green값, blue값, 투명도)
    ```
    
    - rgba() 함수의 red, green, blue는 0 ~ 255 값을 가질 수 있다.
        - 투명도는 0.0(투명) ~ 1.0(불투명) 값을 가진다.
    - ex) 경계선을 빨간색, 굵기를 1픽셀
        
        ```css
        -fx-border-color: red;
        -fx-border-width: 1;
        ```
        
        ![Untitled](/images/lang_java/javaFx/border_속성/Untitled.png)
        
    - ex) 둥근 모서리
        - -fx-border-radius 속성은 원의 반지름을 설정해서 둥근 모서리를 만든다.
        
        ```css
        -fx-border-color: red;
        -fx-border-width: 1;
        -fx-border-radius: 20;
        ```
        
        ![Untitled](/images/lang_java/javaFx/border_속성/Untitled%201.png)
        
    - ex) JavaFX 컨테이너 및 컨트롤은 바깥 경계선 외에 내부 경계선을 여러 개 정의 가능
        - -fx-border-insets 속성으로 경계선이 나타날 깊이를 쉼표로 구분해서 나열할 경우
            - 다른 속성들도 경계선의 개수에 맞게 굵기, 색상, 스타일을 지정할 수 있다.
        - 다음은 3개의 경계선을 정의하고 각각 색상 및 굵기를 생성
        
        ```css
        -fx-border-insets: 0, 10, 20;
        -fx-border-color: red, green, blue;
        -fx-border-width: 1, 1, 1;
        ```
        
        ![Untitled](/images/lang_java/javaFx/border_속성/Untitled%202.png)
        
    - ex) top, right, bottom, left 별로 굵기, 색상, 스타일 지정 가능
        - 값의 순서는 top부터 시작하여 시계 방향으로 top, right, bottom, left로 나열
        - 두 번째 경계선 top, bottom의 색상을 녹색으로 설정
        - 세 번째 경계선 right, bottom의 굵기를 3으로 설정
        
        ```css
        -fx-border-insets: 0, 10, 20;
        -fx-border-color: red, green white green white, blue;
        -fx-border-width: 1, 1, 1 3 3 1;
        ```
        
        ![Untitled](/images/lang_java/javaFx/border_속성/Untitled%203.png)
        
    - ex) -fx-border-style 속성은 실선과 점선을 설정
        - solid(실선), dotted(점선), dashed(대시선)와 선의 길이 및 공백을 설정할 수 있는 `segments()`를 값으로 줄 수 있다.
        - `segments()`의 매개값
            - 홀수 번째 값은 선의 길이
            - 짝수 번째 값은 공백의 길이
        - 바깥 경계선의 top, right, bottom, left를 다른 스타일로 설정
        
        ```css
        -fx-border-color: red;
        -fx-border-width: 2;
        -fx-border-style: solid dotted dashed segments(3, 2, 8, 2);
        ```
        
        ![Untitled](/images/lang_java/javaFx/border_속성/Untitled%204.png)
        
- ex) FXML로 다섯 개의 VBox를 배치
    - border 속성을 다르게 적용
    - 4개의 VBox 배치
        - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.layout.HBox?>
    <?import javafx.scene.layout.VBox?>
    
    <HBox alignment="CENTER" spacing="20.0" 
    	  xmlns="http://javafx.com/javafx/19" 
    	  xmlns:fx="http://javafx.com/fxml/1">
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
       <children>
          <VBox id="vBox1" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox2" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox3" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox4" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox5" prefHeight="100.0" prefWidth="100.0" />
       </children>
    </HBox>
    ```
    
    - CSS 파일
        - app.css
    
    ```css
    #vBox1 {
    	-fx-border-color: red;
    	-fx-border-width: 1;
    }
    
    #vBox2 {
    	-fx-border-color: red;
    	-fx-border-width: 1;
    	-fx-border-radius: 20;
    }
    
    #vBox3 {
    	-fx-border-insets: 0, 10, 20;
    	-fx-border-color: red, green, blue;
    	-fx-border-width: 1 1 1;
    }
    
    #vBox4 {
    	-fx-border-insets: 0, 10, 20;
    	-fx-border-color: red, green white, green white green white, blue;
    	-fx-border-width: 1, 1, 1 3 3 1;
    }
    
    #vBox5 {
    	-fx-border-color: red;
    	-fx-border-width: 2;
    	-fx-border-style: solid dotted dashed segments(3, 2, 8, 2);
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
    		Parent root = (Parent) FXMLLoader
    				.load(getClass().getResource("Root.fxml"));
    		root.getStylesheets().add(getClass()
    				.getResource("app.css").toString());
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
    
    ![Untitled](/images/lang_java/javaFx/border_속성/Untitled%205.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판