---
title: "[Java] JavaFX CSS 스타일, font 속성"
description: ""
date: "2022-12-27T13:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
  - "CSS"
---
<!--more-->

- 글자의 스타일을 설정
    - 폰트 크기, 종류, 굵기, 색상 등을 설정할 수 있는 세부 속성을 가지고 있다.
    
    | 속성 | 설명 |
    | --- | --- |
    | -fx-font-size | 폰트 크기 |
    | -fx-font-family | 폰트 종류 |
    | -fx-font-weight | 폰트 굵기(blod) |
    | -fx-text-fill | 폰트 색상(단색, 선형 그라디언트, 원형 그라디언트) |
- ex) Label 컨트롤의 폰트를 설정
    - 폰트 종류 : Arial Black
    - 크기 : 35
    - bold
    - 색상 : 선형 그라디언트
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.layout.VBox?>
    
    <VBox id="root" xmlns:fx="http://javafx.com/fxml/1" xmlns="http://javafx.com/javafx/19">
       <children>
          <Label id="welcome-text" text="Welcome" />
       </children>
    </VBox>
    ```
    
    - app.css
    
    ```css
    #root {
    	-fx-padding: 10;
    }
    
    #welcome-text {
    	-fx-font-size: 35;
    	-fx-font-family: "Arial Black";
    	-fx-font-weight: bold;
    	-fx-text-fill: linear-gradient(to bottom, blue, white);
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/font_속성/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판