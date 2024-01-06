---
title: "[Java] JavaFX, StackPane 컨테이너"
description: ""
date: "2022-12-16T20:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- 컨트롤을 겹쳐 배치하는 컨테이너
- 흔히 카드 레이아웃이라고 한다.
    - 카드가 겹쳐져 있는 것처럼 컨트롤도 겹쳐질 수 있다.
    
    ![Untitled](/images/lang_java/javaFx/StackPane_컨테이너/Untitled.png)
    
    - 만약 위에 있는 컨트롤이 투명이라면 밑에 있는 컨트롤이 겹쳐 보이게 된다.
- StackPane은 화면 이동을 하기 위해 사용되기도 한다.
- ex) StackPane 컨테이너
    - 두 개의 ImageView를 StackPane에 겹치도록 배치
    - 하단 이미지는 설경, 상단 이미지는 JavaFX 이다.
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.image.Image?>
    <?import javafx.scene.image.ImageView?>
    <?import javafx.scene.layout.StackPane?>
    
    <StackPane maxHeight="-Infinity" maxWidth="-Infinity" minHeight="-Infinity" minWidth="-Infinity" prefHeight="400.0" prefWidth="600.0" xmlns="http://javafx.com/javafx/19" xmlns:fx="http://javafx.com/fxml/1">
       <children>
          <ImageView fitHeight="300.0" fitWidth="500.0" pickOnBounds="true" preserveRatio="true">
             <image>
                <Image url="@images/winter.jpg" />
             </image>
          </ImageView>
          <ImageView fitHeight="150.0" fitWidth="200.0" pickOnBounds="true" preserveRatio="true">
             <image>
                <Image url="@images/javafx.jpg" />
             </image>
          </ImageView>
       </children>
    </StackPane>
    ```
    
    ![Untitled](/images/lang_java/javaFx/StackPane_컨테이너/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판