---
title: "[Java] JavaFX, BorderPane 컨테이너"
description: ""
date: "2022-12-15T22:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- BorderPane은 top, bottom, left, right, center 셀에 컨트롤을 배치하는 컨테이너
- 컨트롤만 배치하는 것이 아니라 다른 컨테이너도 배치할 수 있기 때문에 다양한 레이아웃을 만들어 낼 수 있다.
    - 주의할 점은 각 셀에는 하나의 컨트롤 또는 컨테이너만 배치할 수 있다.
    
    ![Untitled](/images/lang_java/javaFx/BorderPane_컨테이너/Untitled.png)
    
    ### BorderPane에서 사용할 수 있는 태그 및 속성들
    
    | 태그 및 속성 | 설명 | 적용 |
    | --- | --- | --- |
    | preWidth | 폭을 설정 | BorderPane |
    | preHeight | 높이를 설정 | BorderPane |
    | top | top에 배치될 컨트롤을 포함 | BorderPane |
    | bottom | bottom에 배치될 컨트롤을 포함 | BorderPane |
    | right | right에 배치될 컨트롤을 포함 | BorderPane |
    | left | left에 배치될 컨트롤을 포함 | BorderPane |
    | center | center에 배치될 컨트롤을 포함 | BorderPane |
- BorderPane의 특징은 top, bottom, left, right에 컨트롤을 배치하지 않으면 center에 배치된 컨트롤이 top, bottom, left, right까지 확장된다.
- ex) BorderPane 컨테이너
    - top에는 ToolBar를 배치
    - center에는 TextArea를 배치
    - bottom에는 다시 BorderPane을 배치
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.TextArea?>
    <?import javafx.scene.control.TextField?>
    <?import javafx.scene.control.ToolBar?>
    <?import javafx.scene.layout.BorderPane?>
    
    <BorderPane maxHeight="-Infinity" maxWidth="-Infinity" minHeight="-Infinity" minWidth="-Infinity" prefHeight="400.0" prefWidth="600.0" xmlns="http://javafx.com/javafx/19" xmlns:fx="http://javafx.com/fxml/1">
       <top>
          <ToolBar BorderPane.alignment="CENTER">
            <items>
              <Button mnemonicParsing="false" text="Button" />
                <Button mnemonicParsing="false" text="Button" />
            </items>
          </ToolBar>
       </top>
       <center> <!-- left와 right까지 확장 -->
          <TextArea BorderPane.alignment="CENTER" />
       </center>
       <bottom>
          <BorderPane BorderPane.alignment="CENTER">
             <center> <!-- top, bottom, left까지 확장 -->         
                <TextField BorderPane.alignment="CENTER" />
             </center>
             <right>
                <Button mnemonicParsing="false" text="Button" BorderPane.alignment="CENTER" />
             </right>
          </BorderPane>
       </bottom>
    </BorderPane>
    ```
    
    ![Untitled](/images/lang_java/javaFx/BorderPane_컨테이너/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판