---
title: "[Java] JavaFX CSS 스타일, 외부 CSS 파일"
description: ""
date: "2022-12-27T10:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
  - "CSS"
---
<!--more-->

- 인라인 스타일은 컨테이너와 컨트롤의 style 속성으로 CSS를 직접 적용
    - 동일한 스타일을 적용하는 컨테이너와 컨트롤이 많아질수록 중복 코드가 늘어나는 단점
    - 레이아웃과 CSS가 뒤섞여 추후 유지 보수가 어려움
- 인라인 방법보다는 중복 코드를 줄이고, 재사용성을 높이면서 유지보수도 편리한 외부 CSS 파일을 작성해서 적용하는 것이 일반적

## 1. 선택자

- 외부 CSS 파일은 스타일을 적용할 컨테이너와 컨트롤을 선택해주는 선택자가 필요
    - ex) 선택자를 작성하는 방법
    
    ```css
    선택자 {
    	속성:값; 속성:값; ...
    }
    ```
    
- 선택자는 중괄호 `{}` 에 정의된 CSS 속성을 적용할 컨테이너 또는 컨트롤을 선택하는 역할
    
    
    | 선택자 | 작성 방법 |
    | --- | --- |
    | Type 선택자 | Type {속성:값; 속성:값;} |
    | id 선택자 | #id {속성:값; 속성:값;} |
    | class 선택자 | .class {속성:값; 속성:값;} |
    - Type 선택자는 같은 타입의 컨테이너 또는 컨트롤을 모두 선택
        - ex) 모든 Label 컨트롤의 안쪽 여백을 5만큼 주고 싶다면 다음과 같이 정의
            
            ```xml
            <Label ... >
            <Label ... >
            ```
            
            ```css
            Label {
            	-fx-padding: 5;
            }
            ```
            
    - #id 선택자는 동일한 id 속성값을 가지고 있는 컨테이너 또는 컨트롤을 선택
        - ex) id 속성값이 lblId인 Label의 배경을 검은색으로, 글자를 노란색으로 설정
            
            ```xml
            <Label id="lblId">
            ```
            
            ```css
            #lblId {
            	-fx-background-color: black;
            	-fx-text-fill: yellow;
            }
            ```
            
    - .class 선택자는 동일한 styleClass 속성값을 가지고 있는 컨테이너 또는 컨트롤을 선택
        - ex) styleClass 속성값이 lblClass인 Label의 배경을 파란색으로, 글자를 흰색으로 설정
            
            ```xml
            <Label styleClass="lblClass">
            <Label styleClass="lblClass">
            ```
            
            ```css
            .lblClass {
            	-fx-background-color: blue;
            	-fx-text-fill: white;
            }
            ```
            
    - FXML 파일에서 id 속성은 유일한 값을 가져야 하지만, styleClass 속성은 중복된 값을 가질 수 잇다.
        - id 선택자는 하나의 컨트롤만 선택
        - class  선택자는 동시에 여러 컨트롤을 선택 가능
    - Type 선택자와 class 선택자는 조합이 가능
        - ex) Label 컨트롤 중에서 `styleClass=”className”`을 가진 것만 CSS를 적용
            
            ```css
            Label.className {
            	-fx-background-color: blue;
            	-fx-text-fill: white;
            }
            ```
            
- 컨트롤은 세 가지 상태를 가질 수 있다.
    - 입력 가능한 상태(focused)
    - 마우스가 위에 있는 상태(hover)
    - 마우스를 클릭한 상태(pressed)
    
    각 상태에 따라 스타일을 다르게 적용할 경우
    
    - 선택자 다음에 `:focused`, `:hover`, `:pressed`를 붙이면 된다.
    - 이러한 것들을 유사 클래스(pseudo-class)라고 한다.
    
    ### 유사 클래스(pseudo-class)
    
    | 상태 | 상태별 선택자 |
    | --- | --- |
    | 입력 가능한 상태 | 선택자:focused {속성:값; 속성:값; …} |
    | 마우스가 컨트롤 위에 있는 상태 | 선택자:hover {속성:값; 속성;값; …} |
    | 마우스로 컨트롤을 누른 상태 | 선택자:pressed {속성:값; 속성:값; …} |

## CSS 파일 적용

- 외부 CSS 파일은 개별 컨테이너 또는 컨트롤에 적용하거나 Scene에 추가하여 Scene 내부의 모든 컨테이너와 컨트롤에 적용 가능
- 컨테이너 또는 컨트롤에 CSS 파일을 적용하려면 FXML 파일에서 해당 태그의 stylesheets 속성으로 CSS 파일 경로를 지정
    
    ```xml
    <컨테이너 stylesheets="@app.css">
    ```
    
- Scene에 적용하려면 Java 코드로 Scene의 `getStylesheets()` 메소드를 호출, ObservableList를 얻고 CSS 파일 경로를 추가
    
    ```java
    scene.getStylesheets()
         .add(getClass().getResource("app.css").toString());
    ```
    
- ex) 실행 클래스
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
    		Parent root = (Parent) FXMLLoader.load(getClass()
    				.getResource("Root.fxml"));
    		Scene scene = new Scene(root);
    		scene.getStylesheets().add(getClass()
    				.getResource("app.css").toString());
    		
    		primaryStage.setTitle("AppMain");
    		primaryStage.setScene(scene);
    		primaryStage.show();
    	}
    
    	public static void main(String[] args) {
    		launch(args);
    	}
    }
    ```
    
- ex) 세 개의 Label에 CSS를 적용해서 배경색과 글자색을 변경
    - 세 가지 선택자를 사용
    - 첫 번째 Label은 id 속성
    - 두 번째와 세 번째 Label은 styleClass 속성
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.layout.HBox?>
    
    <HBox alignment="CENTER" prefHeight="50.0" 
    	  prefWidth="400.0" spacing="20.0" 
    	  xmlns:fx="http://javafx.com/fxml/1" 
    	  xmlns="http://javafx.com/javafx/19">
       <children>
          <Label id="lblId" text="검정바탕,노란글씨" />
          <Label styleClass="lblClass" text="파란바탕,흰글씨" />
          <Label styleClass="lblClass" text="파란바탕,흰글씨" />
       </children>
    </HBox>
    ```
    
    - 모든 Label은 안쪽 여백을 5로 가져야 되면, id가 lblId인 Label은 배경을 검은색, 글자는 노란색으로 적용
        - styleClass가 lblClass인 모든 Label은 배경을 파란색, 글자는 흰색으로 적용
    - app.css
    
    ```css
    /* 전체 Label 선택 */
    Label {
    	-fx-padding: 5;
    }
    
    /* id="IblId"을 가진 컨트롤 선택 */
    #lblId {
    	-fx-background-color: black;
    	-fx-text-fill: yellow;
    }
    
    /* styleClass="lblClass"을 가진 컨트롤 선택 */
    .lblClass {
    	-fx-background-color: blue;
    	-fx-text-fill: white;
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/외부_CSS_파일/Untitled.png)
    
- ex) TextField와 Button 컨트롤의 상태에 따라서 스타일을 변경
    - TextField가 입력 가능한 상태가 되면 배경색을 노란색으로 변경
    - 마우스가 Button 위에 있으면 배경을 노란색으로 변경
        - 클릭하면 빨간색으로 변경
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.TextField?>
    <?import javafx.scene.layout.HBox?>
    
    <HBox spacing="10.0" 
    	  xmlns:fx="http://javafx.com/fxml/1" 
    	  xmlns="http://javafx.com/javafx/19">
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
       <children>
          <TextField prefWidth="200.0" />
          <Button mnemonicParsing="false" prefWidth="50.0" text="확인" />
       </children>
    </HBox>
    ```
    
    - app.css
    
    ```css
    TextField:focused {
    	-fx-border-color: skyblue;
    	-fx-background-color: yellow;
    }
    
    Button:hover {
    	-fx-border-color: skyblue;
    	-fx-background-color: yellow;
    }
    
    Button:pressed {
    	-fx-border-color: skyblue;
    	-fx-background-color: red;
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/외부_CSS_파일/Untitled%201.png)
    
    ![Untitled](/images/lang_java/javaFx/외부_CSS_파일/Untitled%202.png)
    
    ![Untitled](/images/lang_java/javaFx/외부_CSS_파일/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판