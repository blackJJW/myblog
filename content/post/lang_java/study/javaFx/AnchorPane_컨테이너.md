---
title: "[Java] JavaFX, AnchorPane 컨테이너"
description: ""
date: "2022-12-15T22:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- AnchorPane 컨테이너는 좌표를 이용하여 AnchorPane의 좌상단 (0, 0)을 기준으로 컨트롤을 배치
- 컨트롤 좌표는 좌상단(layoutX, layoutY) 값을 말한다.
    - (0, 0)에서 떨어진 거리
    
    ![Untitled](/images/lang_java/javaFx/AnchorPane_컨테이너/Untitled.png)
    
    ### AnchorPane에서 사용할 수 있는 주요 설정
    
    | 태그 및 속성     | 설명 | 적용 |
-------------| --- | --- | --- |
    | prefWidth   | 폭을 설정 | AnchorPane |
    | prefHeight  | 높이를 설정 | AnchorPane |
    | layoutX     | 컨트롤의 X 좌표 | 컨트롤 |
    | layoutY     | 컨트롤의 Y 좌표 | 컨트롤 |
    | children | 컨트롤을 포함 | AnchorPane |
- ex) AnchorPane 컨테이너
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.control.PasswordField?>
    <?import javafx.scene.control.TextField?>
    <?import javafx.scene.layout.AnchorPane?>
    
    <AnchorPane maxHeight="-Infinity" maxWidth="-Infinity" minHeight="-Infinity" minWidth="-Infinity" prefHeight="150.0" prefWidth="300.0" xmlns="http://javafx.com/javafx/19" xmlns:fx="http://javafx.com/fxml/1">
       <children>
          <Label layoutX="42.0" layoutY="28.0" text="아이디" />
          <Label layoutX="42.0" layoutY="66.0" text="패스워드" />
          <TextField layoutX="120.0" layoutY="24.0" />
          <PasswordField layoutX="120.0" layoutY="62.0" />
          <Button layoutX="97.0" layoutY="106.0" mnemonicParsing="false" text="로그인" />
          <Button layoutX="164.0" layoutY="106.0" mnemonicParsing="false" text="취소" />
       </children>
    </AnchorPane>
    ```
    
    - Main.java
    
    ```java
    import javafx.application.Application;
    import javafx.fxml.FXMLLoader;
    import javafx.scene.Parent;
    import javafx.scene.Scene;
    import javafx.stage.Stage;
    
    public class Main extends Application {
    	@Override
    	public void start(Stage primaryStage) throws Exception {
    		Parent parent = FXMLLoader.load(getClass().getResource("Root.fxml"));
    		Scene scene = new Scene(parent);
    		
    		primaryStage.setTitle("Main");
    		primaryStage.setScene(scene);
    		primaryStage.show();
    		
    	}
    	
    	public static void main(String[] args) {
    		launch(args);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/AnchorPane_컨테이너/Untitled%201.png)
    
- AnchorPane에 포함될 컨트롤은 `<children>` 태그의 자식 태그로 선언
    - `<children>`은 AnchorPane의 `setChildren()` 메소드를 호출하는 것이 아니라, `getChildren()` 메소드가 리턴하는 ObservableList 컬렉션에 `<children>`의 자식 태그로 정의된 컨트롤을 추가하는 역할
    - AnchorPane에서만 `<children>` 태그를 사용하는 것이 아니라, 거의 모든 컨테이너에서 사용
- AnchorPane을 사용해서 컨트롤을 좌표로 배치하면 윈도우 창이 줄거나 늘어날 경우 컨트롤의 재배치가 일어나지 않는다.
    - 따라서 AnchorPane으로 배치할 경우에는 윈도우 창의 크기를 변경할 수 없도록 Stage의 `setResizable(false);` 메소드를 호출하는 것이 좋다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판