---
title: "[Java] JavaFX, GridPane 컨테이너"
description: ""
date: "2022-12-15T23:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- 그리드로 컨트로을 배치하되 셀의 크기가 고정적이지 않고 유동적인 컨테이너
- 셀 병합이 가능하기 때문에 다양한 입력폼 화면을 만들 때 매우 유용하게 사용 가능
- 각 컨트롤은 자신이 배치될 행 인덱스와 컬럼 인덱스를 속성으로 가지며, 몇 개의 셀을 병합할 것인지도 지정 가능
    
    ![Untitled](/images/lang_java/javaFx/GridPane_컨테이너/Untitled.png)
    
    ### GridPane에 적용 가능한 속성
    
    | 태그 및 속성 | 설명 | 적용 |
    | --- | --- | --- |
    | prefWidth | 폭을 설정 | GridPane |
    | prefHeight | 높이를 설정 | GridPane |
    | hgap | 수평 컨트롤 간격을 설정 | GridPane |
    | vgap | 수직 컨트롤 간격을 설정 | GridPane |
    | children | 컨트롤을 포함 | GridPane |
    | GridPane.rowIndex | 컨트롤이 위치하는 행 인덱스를 설정 | 컨트롤 |
    | GridPane.columnIndex | 컨트롤이 위치하는 열 인텍스를 설정 | 컨트롤 |
    | GridPane.rowSpan | 행 병합 수를 설정 | 컨트롤 |
    | GridPane.columnSpan | 컬럼 병합 수를 설정 | 컨트롤 |
    | GridPane.hgrow | 수평 빈 공간 채우기를 설정 | 컨트롤 |
    | GridPane.vgrow | 수직 빈 공간 채우기를 설정 | 컨트롤 |
    | GridPane.halignment | 컨트롤의 수평 정렬을 설정 | 컨트롤 |
    | GridPane.valignment | 컨트롤의 수직 정렬을 설정 | 컨트롤 |
- ex) GridPane 컨테이너
    - 로그인 화면을 GridPane으로 배치
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.control.TextField?>
    <?import javafx.scene.layout.GridPane?>
    <?import javafx.scene.layout.HBox?>
    
    <GridPane hgap="10.0" prefWidth="300.0" vgap="10.0" xmlns:fx="http://javafx.com/fxml/1" xmlns="http://javafx.com/javafx/19">
       <children>
          <Label text="아이디" />
          <Label text="패스워드" GridPane.hgrow="ALWAYS" GridPane.rowIndex="1" />
          <TextField GridPane.columnIndex="1" GridPane.hgrow="ALWAYS" />
          <TextField GridPane.columnIndex="1" GridPane.hgrow="ALWAYS" GridPane.rowIndex="1" />
          <HBox alignment="CENTER" spacing="20.0" GridPane.columnSpan="2" GridPane.hgrow="ALWAYS" GridPane.rowIndex="2">
             <children>
                <Button mnemonicParsing="false" text="로그인" />
                <Button mnemonicParsing="false" text="취소" />
             </children>
          </HBox>
       </children>
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
    </GridPane>
    ```
    
    ![Untitled](/images/lang_java/javaFx/GridPane_컨테이너/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판