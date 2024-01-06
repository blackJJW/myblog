---
title: "[Java] JavaFX, FXML 컨트롤러(Controller)"
description: ""
date: "2022-12-16T21:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- FXML 레이아웃은 FXML 파일당 별도의 컨트롤러(Controller)를 지정해서 이벤트를 처리할 수 있기 때문에 FXML 레이아웃과 이벤트 처리 코드를 완전히 분리 가능

### fx:controller 속성과 컨트롤러 클래스

- FXML 파일의 루트 태그에서 fx:controller 속성으로 컨트롤러를 지정하면 UI 컨트롤에서 발생하는 이벤트를 컨트롤러가 처리
    
    ```xml
    <루트컨테이너 xlmns:fx="http://javafx.com/fxml"
    	fx:controller="packageName.ControllerName">
    	...
    </루트컨테이너>
    ```
    
- 컨트롤러는 Initializable 인터페이스를 구현한 클래스로 작성
    
    ```java
    public class CotrollerName implements Initializable {
    	@Override
    	public void initialize(URL location, ResourceBundle resources) { }
    }
    ```
    
    - `initialize()` 메소드는 컨트롤러 객체가 생성되고 나서 호출
        - 주로 UI 컨트롤의 초기화, 이벤트 핸들러 등록, 속성 감시 등의 코드가 작성된다.

### fx:id 속성과 @FXML 컨트롤 주입

- 컨트롤러는 이벤트 핸들러를 등록하기 위해서, 그리고 이벤트 처리 시 UI를 변경하기 위해 FXML 파일에 포함된 컨테이너 및 컨트롤의 참조가 필요
    - 이를 위해 FXML 파일에 포함된 컨트롤들은 fx:id 속성을 가질 필요가 있다.
- ex) fx:id 속성
    - root.fxml
    
    ```xml
    <HBox xmlns:fx="http://javafx.com/fxml"
    		fx:controller="fxmlController.RootController"
    		prefHeight="50.0" prefWidth="200.0"
    		alignment="CENTER" spacing="20.0"
    	<children>
    		<Button fx:id="btn1" text="버튼1" />
    		<Button fx:id="btn2" text="버튼2" />
    		<Button fx:id="btn3" text="버튼3" />
    	</children>
    </HBox>
    ```
    
    - fx:id 속성을 가진 컨트롤들은 컨트롤러의 `@FXML` 어노테이션이 적용된 필드에 자동 주입
    - 주의할 점은 fx:id 속성값과 필드명은 동일해야 한다.
    
    ```java
    public class ControllerName implements Initializable {
    	@FXML private Button btn1;
    	@FXML private Button btn2;
    	@FXML private Button btn3;
    	@Override
    	public void initializable(URL location, ResourceBundle resources) { } 
    }
    ```
    
- FXMLLoader가 FXML 파일을 로딩할 때, 태그로 선언된 컨트롤 객체가 생성
    - 컨트롤러 객체도 함께 생성
    - @FXML 어노테이션이 적용된 필드에 컨트롤 객체가 자동 주입
    - 주입이 완료되면 비로소 `initialize()` 메소드가 호출되기 때문에 `initialize()` 내부에서 필드를 안전하게 사용할 수 있다.

### EventHandler 등록

- 컨트롤에서 발생하는 이벤트를 처리하려면 컨트롤러의 `initialize()` 메소드에서 EventHandler를 생성하고 등록해야 한다.
- ex) EventHandler 생성 및 등록
    - 세 개의 Button에서 발생하는 ActionEvent를 처리하는 방법
    - RootController.java
    
    ```java
    import java.net.URL;
    import java.util.ResourceBundle;
    
    import javafx.event.ActionEvent;
    import javafx.event.EventHandler;
    import javafx.fxml.FXML;
    import javafx.fxml.Initializable;
    import javafx.scene.control.Button;
    
    public class RootController implements Initializable {
    	@FXML private Button btn1; // btn1 객체 주입
    	@FXML private Button btn2; // btn2 객체 주입
    	@FXML private Button btn3; // btn3 객체 주입
    	
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    		btn1.setOnAction(new EventHandler<ActionEvent>() {
    			@Override
    			public void handle(ActionEvent event) {
    				handleBtn1Action(event);
    			}
    		});
    		
    		btn2.setOnAction( event -> handleBtn2Action(event) );
    		btn3.setOnAction( event -> handleBtn3Action(event) );
    	}
    	
    	public void handleBtn1Action(ActionEvent event) {
    		System.out.println("버튼1 클릭");
    	}
    	
    	public void handleBtn2Action(ActionEvent event) {
    		System.out.println("버튼2 클릭");
    	}
    	
    	public void handleBtn3Action(ActionEvent event) {
    		System.out.println("버튼3 클릭");
    	}
    }
    ```
    
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.*?>
    <?import javafx.scene.layout.*?>
    
    <HBox alignment="CENTER"  
    	  prefHeight="50.0" prefWidth="200.0" spacing="20.0" 
          xmlns="http://javafx.com/javafx/19" xmlns:fx="http://javafx.com/fxml/1" 
          fx:controller="fxmlController.RootController">
       <children>
          <Button fx:id="btn1" mnemonicParsing="false" text="버튼1" />
          <Button fx:id="btn2" mnemonicParsing="false" text="버튼2" />
          <Button fx:id="btn3" mnemonicParsing="false" text="버튼3" />
       </children>
    </HBox>
    ```
    
    ![Untitled](/images/lang_java/javaFx/FXML_컨트롤러(Controller)/Untitled.png)
    
    ![Untitled](/images/lang_java/javaFx/FXML_컨트롤러(Controller)/Untitled%201.png)
    

### 이벤트 처리 메소드 매핑

- 컨트롤러에서 EventHandler를 생성하지 않고도 바로 이벤트 처리 메소드와 연결할 수 있는 방법이 있다.
- Button 컨트롤을 작성할 때 onAction 속성값으로 “#메소드명”을 주면 내부적으로 EventHandler 객체가 생성되기 때문에 컨트롤러에서는 해당 메소드만 작성하면 된다.
- **[ FXML 파일 ]**
    
    ```xml
    <Button fx:id="btn" text="버튼" onAction="#handleButton" />
    ```
    
- **[ Controller 파일 ]**
    
    ```java
    public void handleBtnAction(ActionEvent event) { ... }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판