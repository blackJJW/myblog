---
title: "[Java] JavaFX, FlowPane 컨테이너"
description: ""
date: "2022-12-15T22:50:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- FlowPane은 행으로 컨트롤을 배치하되 공간이 부족하면 새로운 행에 배치하는 컨테이너
    
    ![Untitled](/images/lang_java/javaFx/FlowPane_컨테이너/Untitled.png)
    
    ### FlowPane에서 사용할 수 있는 태그와 속성들
    
    | 태그 및 속성 | 설명 | 적용 |
    | --- | --- | --- |
    | prefWidth | 폭을 설정 | FlowPane |
    | prefHeight | 높이를 설정 | FlowPane |
    | hgap | 컨트롤의 수평 간격을 설정 | FlowPane |
    | vgap | 컨트롤의 수직 간격을 설정 | FlowPane |
    | children | 컨트롤을 포함 | FlowPane |
- ex) FlowPane 컨테이너
    - FlowPane 컨테이너에 여섯 개의 Button을 배치
    - 버튼 간의 수평 간격과 수직 간경을 주기 위해 hgap과 vgap을 10으로 설정
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.layout.FlowPane?>
    
    <FlowPane hgap="10.0" prefHeight="70.0" prefWidth="300.0" vgap="10.0" xmlns:fx="http://javafx.com/fxml/1" xmlns="http://javafx.com/javafx/19">
       <children>
          <Button mnemonicParsing="false" text="Button" />
          <Button mnemonicParsing="false" text="Button" />
          <Button mnemonicParsing="false" text="Button" />
          <Button mnemonicParsing="false" text="Button" />
          <Button mnemonicParsing="false" text="Button" />
          <Button mnemonicParsing="false" text="Button" />
       </children>
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
    </FlowPane>
    ```
    
    ![Untitled](/images/lang_java/javaFx/FlowPane_컨테이너/Untitled%201.png)
    
    - 오른쪽에 배치될 공간이 부족할 경우에는 새로운 행에 컨트롤이 배치된다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판