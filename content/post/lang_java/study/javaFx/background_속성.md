---
title: "[Java] JavaFX CSS 스타일, background 속성"
description: ""
date: "2022-12-27T12:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
  - "CSS"
---
<!--more-->

- background 속성은 컨테이너 및 컨트롤의 배경 스타일을 설정
    - 배경 색상, 배역 이미지를 설정할 수 있는 세부 속성을 가지고 있다.
    
    | 속성 | 설명 |
    | --- | --- |
    | -fx-background-color | 배경 색상 |
    | -fx-background-image | 배경 이미지 |
    | -fx-background-position | 배경 이미지 위치(top, right, bottom, left, center) |
    | -fx-background-repeat | 이미지 반복 여부(no-repeat: 반복 X) |
- -fx-background-color 속성은 배경 색상을 설정
    - 단일 색상을 지정하는 방법
    
    ```css
    -fx-background-color: red;      //색 이름
    -fx-background-color: ##ff0000; // #색상번호
    -fx-background-color: rgba(255, 0, 0, 0); 
                        // rgba(red값, green값, blue값, 투명도)
    ```
    
    - 선형 그라디언트 설정
        - 선형 그라디언트는 시작 색에서부터 끝 색까지 진행 방향으로 서서히 색상 변화를 준다.
        
        ```css
        linear-gradient(to 진행방향, 시작색 S%, 중간색 M%, ..., 끝색);
        ```
        
        - 진행 방향은 `to bottom`, `to right`, `to bottom right` 등과 같이 밑으로, 오른쪽으로, 대각선으로 지정 가능
        - 각각의 색은 몇 % 정도가 나올지 S, M 값으로 지정 가능
            - 단, 끝 색은 % 값을 줄 수 없다.
            - % 값이 생략되면 균등하게 색상이 나온다.
        
        ex) 선형 그라디언트 적용
        
        ```css
        -fx-background-color: linear-gradient(to right, black, white);
        ```
        
        ![Untitled](/images/lang_java/javaFx/background_속성/Untitled.png)
        
    - 원형 그라디언트 설정
        - 시작 색에서부터 끝 색까지 원형으로 서서히 색상 변화를 준다.
        
        ```css
        radial-gradient(center X% Y%, radius R%, 시작색 S%, 중간색 M%, 끝색);
        ```
        
        - X%와 Y%는 컨테이너 및 컨트롤의 좌상단을 0%, 0%로 보고, 원의 중심점이 위치하는 곳을 지정
            
            ex) center 50% 50%는 원의 정중앙을 중심점으로 설정
            
        - R%는 중심점에서부터 색상 변화가 종료되는 위치
            
            ex) radius 50%는 컨테이너 및 컨트롤 전체 영역에 원형 그라디언트를 적용
            
        - 각각의 색이 몇 % 정도로 나올지 S, M 값으로 지정 가능
            - 단, 끝 색은 % 값을 줄 수 없다.
            - % 값이 생략되면 균등하게 색상이 나온다.
        
        ex) 원형 그라디언트 적용
        
        ```css
        -fx-background-color: radial-gradient(center 50% 50%, radius 50%, #ffffff 10%, #000000);
        ```
        
        ![Untitled](/images/lang_java/javaFx/background_속성/Untitled%201.png)
        
- -fx-background-image 속성은 배경 이미지를 설정
    - 배경 이미지보다 컨테이너 및 컨트롤의 사이즈가 더 크면 이미지는 반복적으로 드로잉된다.
        
        ![Untitled](/images/lang_java/javaFx/background_속성/Untitled%202.png)
        
    - 한 번만 드로잉하려면 -fx-background-repeat 속성값을 no-repeat로 설정
        
        ![Untitled](/images/lang_java/javaFx/background_속성/Untitled%203.png)
        
    - 이미지의 위치는 -fx-background-position으로 설정
        
        ```css
        -fx-background-image: url("이미지 경로"); // 이미지 파일 경로
        -fx-background-repeat: no-repeat; // 한 번만 드로잉
        -fx-background-position: center; // 정중앙에 위치
        ```
        
        ![Untitled](/images/lang_java/javaFx/background_속성/Untitled%204.png)
        
- ex) 6개의 VBox에 차례대로 단색, 선형 그라디언트, 원형 그라디언트, 반복 이미지, 단일 이미지, 정중앙 단일 이미지를 적용
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.layout.HBox?>
    <?import javafx.scene.layout.VBox?>
    
    <HBox spacing="10.0" xmlns="http://javafx.com/javafx/19" xmlns:fx="http://javafx.com/fxml/1">
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
       <children>
          <VBox id="vBox1" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox2" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox3" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox4" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox5" prefHeight="100.0" prefWidth="100.0" />
          <VBox id="vBox6" prefHeight="100.0" prefWidth="100.0" />
       </children>
    </HBox>
    ```
    
    - app.css
    
    ```css
    #vBox1 {
    	-fx-background-color: rgba(0, 0, 0, 1);
    }
    
    #vBox2 {
    	-fx-background-color: linear-gradient(to right, #000000, #ffffff);
    }
    
    #vBox3 {
    	-fx-background-color: radial-gradient(center 50% 50%, radius 50%, #ffffff, #000000);
    }
    
    #vBox4 {
    	-fx-border-color: skyblue;
    	-fx-background-image: url("images/user.png");
    }
    
    #vBox5 {
    	-fx-border-color: skyblue;
    	-fx-background-image: url("images/user.png");
    	-fx-background-repeat: no-repeat;
    }
    
    #vBox6 {
    	-fx-border-color: skyblue;
    	-fx-background-image: url("images/user.png");
    	-fx-background-repeat: no-repeat;
    	-fx-background-position: center;
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/background_속성/Untitled%205.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판