---
title: "[Java] JavaFX, HBox와 VBox 컨테이너"
description: ""
date: "2022-12-15T22:35:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- HBox와 VBox는 수평과 수직으로 컨트롤을 배치하는 컨테이너
    - 자식 컨트롤의 크기를 재조정
    - HBox는 컨트롤의 높이를 확장
        - 컨트롤의 폭은 유지
    - VBox는 컨트롤의 폭을 확장
        - 컨트롤의 높이는 유지
    
    ![Untitled](/images/lang_java/javaFx/HBox와_VBox_컨테이너/Untitled.png)
    
    - 크기 조정이 가능한 컨트롤만 자동 확장
    - Button의 경우는 크기 조정이 되지 않는다.
        - 그 이유는 maxWidth와 maxHeight가 -1.0을 가지기 때문
        - 크기 조정이 가능하도록 하려면 다음과 같이 maxWidth와 maxHeight를 변경하면 된다.
            
            ```xml
            <Button text="button">
            	<maxWidth><Double fx:constant="MAX_VALUE"/></maxWidth>
            	<maxWidth><Double fx:constant="MAX_VALUE"/></maxHeight>
            </Button>
            ```
            
- HBox에서 컨트롤의 높이를 확장하고 싶지 않다면 fillHeight 속성을 false로 설정
- VBox에서 컨트롤의 폭을 확장하고 싶지 않다면 fillWidth 속성을 false로 설정
    
    ### HBox와 VBox에서 사용할 수 있는 주요 설정
    
    ![Untitled](/images/lang_java/javaFx/HBox와_VBox_컨테이너/Untitled%201.png)
    
- ex) HBox와 VBox 컨테이너
    - VBox로 ImageView 컨트롤과 HBox 컨테이너를 수직으로 배치
    - HBox 안에는 두 개의 버튼을 수평으로 배치
    - [다음] 버튼은 HBox의 남은 폭을 채우도록 HBox의 hgrow 속성을 설정
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.image.Image?>
    <?import javafx.scene.image.ImageView?>
    <?import javafx.scene.layout.HBox?>
    <?import javafx.scene.layout.VBox?>
    
    <VBox spacing="20.0" xmlns="http://javafx.com/javafx/19" xmlns:fx="http://javafx.com/fxml/1">
    	<padding>
    		<Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
    	</padding>
    	
    	<children>
    		<ImageView fitWidth="200.0" preserveRatio="true">
    		                         <!-- 그림의 비율에 맞게 높이를 설정 -->
    			<image>
    				<Image url="@images/javafx.jpg" />
    				          <!-- 현재 경로 기준으로 상대경로로 파일 지정 -->
    			</image>
    		</ImageView>
          <HBox alignment="CENTER" spacing="20.0">
             <children>
                <Button mnemonicParsing="false" text="이전" />
                <Button maxWidth="1.7976931348623157E308" mnemonicParsing="false" text="다음" HBox.hgrow="ALWAYS" />
             </children>
          </HBox>
    
    	</children>
    </VBox>
    ```
    
    ![Untitled](/images/lang_java/javaFx/HBox와_VBox_컨테이너/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판