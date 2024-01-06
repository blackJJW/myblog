---
title: "[Java] JavaFX CSS 스타일, shadow 효과"
description: ""
date: "2022-12-28T13:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
  - "CSS"
---
<!--more-->

- 그림자 효과를 주기 위해 -fx-effect 속성을 제공
- 속성값으로는 `dropshadow()`와 `innershadow()`를 줄 수 있다.
    - `dropshadow()` : 바깥 그림자를 주어 튀어 나오는 느낌
    - `innershadow()` : 안쪽 그림자를 주어 움푹 들어간 느낌
    
    ```css
    -fx-effect: dropshadow(three-pass-box, 그림자색상, radius, spread, offsetX, offsetY);
    -fx-effect: innershadow(three-pass-box, 그림자색상, radius, spread, offsetX, offsetY);
    ```
    
    - three-pass-box : blur-type의 종류
        - gaussian, one-pass-box, two-pass-box, three-pass-box 등이 있다.
        - JavaFX는 three-pass-box를 기본으로 사용
    - radius는 blur kernel의 반지름이 0.0 ~ 127.0 사이의 값을 가짐
        - 기본값은 10
    - spread와 choke는 각각 그림자의 spread와 choke로 0.0 ~ 1.0 사이의 값을 가짐
        - 기본값은 0.0
    - offsetX, offsetY : 그림자의 편차
        
        ![Untitled](/images/lang_java/javaFx/shadow_효과/Untitled.png)
        
- ex) 두 개의 버튼에 shadow 효과 적용
    - 첫 번째 버튼에 `dropshadow()`
    - 두 번째 버튼에 `innershadow()`
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.layout.HBox?>
    
    <HBox alignment="CENTER" fillHeight="false" prefHeight="150.0" prefWidth="300.0" spacing="50.0" xmlns:fx="http://javafx.com/fxml/1" xmlns="http://javafx.com/javafx/19">
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
       <children>
          <Button id="btn1" mnemonicParsing="false" prefHeight="50.0" prefWidth="100.0" text="DropShadow" />
          <Button id="btn2" mnemonicParsing="false" prefHeight="50.0" prefWidth="100.0" text="InnerShadow" />
       </children>
    </HBox>
    ```
    
    - app.css
    
    ```css
    #btn1 {
    	-fx-effect: dropshadow(three-pass-box, rgba(0, 0, 0, 0.7), 10, 0, 5, 5);
    }
    
    #btn2 {
    	-fx-effect: innershadow(three-pass-box, rgba(0, 0, 0, 0.7), 10, 0, 5, 5.1);
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/shadow_효과/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판