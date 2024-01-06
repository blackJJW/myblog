---
title: "[Java] JavaFX, Bindings 클래스"
description: ""
date: "2022-12-17T13:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- 두 속성이 항상 동일한 값과 타입을 가질 수는 없다.
    - 한쪽 속성값이 다른쪽 속성값과 동일해지기 위해서는 연산 작업이 필요해질 수도 있다.
    
    ex) 윈도우의 크기에 상관없이 항상 화면 정중앙에 원을 그린다고 가정
    
    - 루트 컨테이너 폭의 1/2이 원의 X좌표
    - 루트 컨테이너 높이의 1/2이 원의 Y좌표
    - 루트 컨테이너의 폭과 높이를 원의 중심과 바인딩하기 위해서는 1/2라는 연산이 필요
    - 이 때 사용할 수 있는 것이 Bindings 클래스가 제공하는 정적 메소드
        - 속성을 연산
        - 다른 타입으로 변환한 후 바인딩하는 기능
    
    ### Bindings 클래스가 제공하는 정적 메소드
    
    | 메소드 | 설명 |
    | --- | --- |
    | add, substract, multiply, divide | 속성값에 덧셈, 곱셈, 나눗셈 연산을 수행하고 바인딩 |
    | max, min | 속성값과 어떤 수를 비교해서 최대, 최소값을 얻고 바인딩 |
    | greaterThan, greaterThanOrEqual | 속성값이 어떤 값보다 큰지, 같거나 큰지를 조사해서 true/false로 변환해서 바인딩 |
    | lessThan, lessThanOrEqual | 속성값이 어떤 값보다 작거나, 같거나 작은지를 조사해서 true/false로 변환해서 바인딩 |
    | equal, notEquals | 속성값이 어떤 값과 같은지, 다른지를 조사해서 true/false로 변환해서 바인딩 |
    | equalIgnoreCase, notEqualIgnoreCase | 대소문자와 상관없이 속성값과 어떤 문자열과 같은지, 다른지를 조사해서 true/false로 변환해서 바인딩 |
    | isEmpty, inNotEmpty | 속성값이 비어있는지, 아닌지를 조사해서 true/false로 변환하여 바인딩 |
    | isNull, isNotNull | 속성값이 null 또는 not null인지를 조사해서 true/false로 변환하여 바인딩 |
    | length | 속성값이 문자열일 경우 문자 수를 얻어 바인딩 |
    | size | 속성 타입이 배열, List, Map, Set일 경우 요소 수를 얻어 바인딩 |
    | and, or | 속성값이 boolean일 경우, 논리곱, 논리합을 얻어 바인딩 |
    | not | 속성값이 boolean일 경우, 반대값으로 바인딩 |
    | convert | 속성값을 문자열로 변환해서 바인딩 |
    | valueAt | 속성이 List, Map일 경우 해당 인덱스 또는 키의 값을 얻어 바인딩 |
- ex) 윈도우 창의 크기가 변경되더라도 항상 화면 정중앙에 원을 그림
    - 루트 컨테이너의 폭과 높이를 원의 중심과 바인딩하기 위해 1/2 연산을 해야 하므로 `Bindings.divide()` 메소드를 이용
    - Circle 배치
        - Root.fxml
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        
        <?import javafx.scene.layout.AnchorPane?>
        <?import javafx.scene.shape.Circle?>
        
        <AnchorPane fx:id="root" 
        			fx:controller="bindingsClass.RootController" 
        			prefHeight="200.0" prefWidth="300.0" 
        			xmlns:fx="http://javafx.com/fxml/1" 
        			xmlns="http://javafx.com/javafx/19">
           <children>
              <Circle fx:id="circle" fill="DODGERBLUE" radius="50.0" stroke="BLACK" />
           </children>
        </AnchorPane>
        ```
        
    - 연산된 속성 바인딩
        - RootController.java
        
        ```java
        import java.net.URL;
        import java.util.ResourceBundle;
        
        import javafx.beans.binding.Bindings;
        import javafx.fxml.FXML;
        import javafx.fxml.Initializable;
        import javafx.scene.layout.AnchorPane;
        import javafx.scene.shape.Circle;
        
        public class RootController implements Initializable {
        	@FXML private AnchorPane root;
        	@FXML private Circle circle;
        	
        	@Override
        	public void initialize(URL location, ResourceBundle resources) {
        		circle.centerXProperty().bind(Bindings.divide(root.widthProperty(), 2));
        		circle.centerYProperty().bind(Bindings.divide(root.heightProperty(), 2));
        		/* circle.centerYProperty() : Circle의 중심 centerY 속성 객체
        		 * Bindings.divide() : 속성 연산
        		 * root.heightProperty() : 루트 컨테이너의 height 속성 객체
        		 * 2 : 나누는 값
        		 */
        	}
        }
        ```
        
    - AppMain.java
        
        ```java
        import javafx.application.Application;
        import javafx.fxml.FXMLLoader;
        import javafx.scene.Parent;
        import javafx.scene.Scene;
        import javafx.stage.Stage;
        
        public class AppMain extends Application {
        	@Override
        	public void start(Stage primaryStage) throws Exception {
        		Parent parent = FXMLLoader.load(getClass().getResource("Root.fxml"));
        		Scene scene = new Scene(parent);
        		
        		primaryStage.setTitle("AppMain");
        		primaryStage.setScene(scene);
        		primaryStage.show();
        	}
        
        	public static void main(String[] args) {
        		launch(args);
        	}
        }
        ```
        
        ![Untitled](/images/lang_java/javaFx/Bindings_클래스/Untitled.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판