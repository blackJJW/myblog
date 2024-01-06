---
title: "[Java] JavaFX CSS 스타일, 인라인(inline) 스타일"
description: ""
date: "2022-12-26T16:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
  - "CSS"
---
<!--more-->

- 컨테이너 또는 컨트롤의 style 속성값으로 직접 CSS를 정의하기 때문에 쉽고, 빠르게 모양과 색상을 변경 가능
    
    ```xml
    <컨테이너 style="속성:값; 속성:값; ... ">
    <컨트롤 style="속성:값; 속성:값; ... ">
    ```
    
- ex) 세 개의 Label은 FXML로 선언
    - style 속성을 이용
        - 첫 번째 Label은 배경을 검은색, 글자는 노란색
        - 두 번째, 세 번째 Label은 배경을 파란색, 글자는 흰색
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.layout.HBox?>
    
    <HBox alignment="CENTER" 
    	  prefHeight="50.0" prefWidth="400.0" spacing="20.0" 
    	  xmlns:fx="http://javafx.com/fxml/1" 
    	  xmlns="http://javafx.com/javafx/19">
       <children>
          <Label text="검정바탕,노란글씨" 
          		style="-fx-background-color: black; 
          			   -fx-text-fill: yellow; 
          			   -fx-padding: 5;"/>
          <Label text="파란바탕,흰글씨" 
          		style="-fx-background-color: blue; 
          			   -fx-text-fill: white; 
          			   -fx-padding: 5;" />
          <Label text="파란바탕,흰글씨" 
             	style="-fx-background-color: blue; 
             		   -fx-text-fill: white; 
             		   -fx-padding: 5;" />
       </children>
    </HBox>
    ```
    
    ![Untitled](/images/lang_java/javaFx/인라인(inline)_스타일/Untitled.png)
    
    - `-fx-background-color` 속성은 배경색을 변경
    - `-fx-text-fill` 속성은 글자색을 변경
    - `-fx-padding` 속성은 안쪽 여백인 패딩을 변경

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판