---
title: "[Java] JavaFX, TilePane 컨테이너"
description: ""
date: "2022-12-15T23:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- 그리드로 컨트롤을 배치하되 고정된 셀(타일) 크기를 갖는 컨테이너
- FlowPane과 마찬가지로 오른쪽에 컨트롤을 배치할 공간이 부족하면 새로운 행에 컨트롤을 배치
    
    ![Untitled](/images/lang_java/javaFx/TilePane_컨테이너/Untitled.png)
    
    ### TilePane에서 사용할 수 있는 태그와 속성
    
    | 태그 및 속성 | 설명 | 적용 |
    | --- | --- | --- |
    | prefWidth | 폭을 설정 | TilePane |
    | prefHeight | 높이를 설정 | TilePane |
    | prefTileWidth | 타일의 폭을 설정 | TilePane |
    | prefTileHeight | 타일의 높이를 설정 | TilePane |
    | children | 컨트롤을 포함 | TilePane |
- ex) TilePane 컨테이너
    - 여러 개의 ImageView를 TilePane에 배치
    - 셀의 크기를 200 x 200으로 지정하기 위해 `prefTileHeight=”200” prefTileWidth=”200”`으로 지정
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.image.Image?>
    <?import javafx.scene.image.ImageView?>
    <?import javafx.scene.layout.TilePane?>
    
    <TilePane prefTileHeight="200.0" prefTileWidth="200.0" xmlns:fx="http://javafx.com/fxml/1" xmlns="http://javafx.com/javafx/19">
       <children>
          <ImageView fitHeight="150.0" fitWidth="200.0" pickOnBounds="true" preserveRatio="true">
             <image>
                <Image url="@images/apple.jpg" />
             </image>
          </ImageView>
          <ImageView fitHeight="150.0" fitWidth="200.0" pickOnBounds="true" preserveRatio="true">
             <image>
                <Image url="@images/melon.jpg" />
             </image>
          </ImageView>
          <ImageView fitHeight="150.0" fitWidth="200.0" pickOnBounds="true" preserveRatio="true">
             <image>
                <Image url="@images/orange.jpg" />
             </image>
          </ImageView>
          <ImageView fitHeight="143.0" fitWidth="200.0">
             <image>
                <Image url="@images/peach.jpg" />
             </image>
          </ImageView>
          <ImageView fitHeight="112.0" fitWidth="200.0">
             <image>
                <Image url="@images/strawberry.jpg" />
             </image>
          </ImageView>
       </children>
    </TilePane>
    ```
    
    ![Untitled](/images/lang_java/javaFx/TilePane_컨테이너/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판