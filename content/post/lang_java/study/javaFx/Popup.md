---
title: "[Java] JavaFX 다이얼로그, Popup"
description: ""
date: "2022-12-26T12:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- Popup은 투명한 컨테이너를 제공하는 모달리스 다이얼로그이다.
    - 소유자 윈도우는 계속 사용 가능
- Popup은 컨트롤의 툴팁(tooltip), 메시지 통지(notification), 드롭 다운 박스(drop down boxes), 마우스 우 클릭 했을 때 나타나는 메뉴 등을 만들 때 사용될 수 있다.
- Popup은 윈도우의 기본 장식(아이콘, 제목, 최소화 및 복원 버튼, 닫기 버튼)을 가지고 있지 않다.
    - Popup의 내용은 자바 코드로 작성하거나, FXML 파일로 작성 가능
    
    ex) FXML 파일을 로딩해서 Popup의 내용으로 추가하는 코드
    
    ```java
    Popup popup = new Popup();
    popup.getContent().add(FXMLLoader.load(getClass()
                                .getResource("Popup.fxml")));
    ```
    
    ex) Popup을 실행하려면 `show()` 메소드를 호출
    
    ```java
    popup.show(primaryStage); // PC 화면 정중앙에서 실행
    popup.show(primaryStage, anchorX, anchorY); 
                              // 지정된 좌표에서 실행
    ```
    
    - anchorX와 anchorY를 지정하지 않으면 PC화면의 중앙에 Popup이 실행
    - 다른 위치에서 실행하고 싶다면 PC화면 좌상단 (0, 0)을 기준으로 anchorX, anchorY를 지정하면 된다.
- Popup은 다른 윈도우보다 최상위에 놓이게 된다.
    - Popup이 실행되면 어떤 윈도우를 실행해도 Popup이 항상 제일 위에 온다.
- Popup을 닫기 위해서는 주 윈도우를 닫거나, Popup 안에 추가된 컨트롤의 이벤트를 처리해서 `hide()` 메소드를 호출해야 한다.
    - 가장 간단한 방법은 `setAutoHide(true)`를 설정하는 것
        - 다른 윈도우로 포커스를 이동하면 Popup은 자동으로 닫힌다.
    
    ```java
    popup.setAutoHide(true);
    ```
    
- ex) 메시지 통지용 Popup을 구현
    - HBox에 CSS를 적용해서 배경을 검은색
        - 모서리는 둥글게 변경
    - Label의 글자색을 흰색으로 변경하기 위해 CSS를 적용
    - 팝업 내용을 정의
        - Popup.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.*?>
    <?import javafx.scene.image.ImageView?>
    <?import javafx.scene.layout.HBox?>
    <?import javafx.geometry.Insets?>
    
    <HBox alignment="CENTER_LEFT" 
    	  xmlns:fx="http://javafx.com/fxml/1" 
    	  xmlns="http://javafx.com/javafx/19"
    	  style="-fx-background-color: black; -fx-background-radius: 20;">
       <children>
          <ImageView id="imgMessage" fitHeight="30.0" fitWidth="30.0" preserveRatio="true" />
          <Label id="lblMessage" style="-fx-text-fill: white;">
          	<HBox.margin>
          		<Insets left="5.0" right="5.0" />
          	</HBox.margin>
          </Label>
       </children>
    </HBox>
    ```
    
    - RootController.java
    
    ```java
    import java.io.File;
    import java.net.URL;
    import java.util.ResourceBundle;
    
    import javafx.event.ActionEvent;
    import javafx.fxml.FXMLLoader;
    import javafx.fxml.Initializable;
    import javafx.scene.control.Label;
    import javafx.scene.image.Image;
    import javafx.scene.image.ImageView;
    import javafx.scene.layout.HBox;
    import javafx.stage.DirectoryChooser;
    import javafx.stage.Popup;
    import javafx.stage.Stage;
    
    public class RootController implements Initializable {
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    	}
    	
    	private Stage primaryStage;
    	public void setPrimaryStage(Stage primaryStage) {
    		this.primaryStage = primaryStage;
    	}
    	
    	// Popup 다이얼로그 실행
    	public void handlePopup(ActionEvent e) throws Exception {
    		Popup popup = new Popup();
    		
    		HBox hbox = (HBox) FXMLLoader.load(getClass().getResource("Popup.fxml"));
    		ImageView imageView = (ImageView) hbox.lookup("#imgMessage");
    		imageView.setImage(
    			new Image(getClass().getResource("images/dialog-info.png").toString())
    		);
        // popup 닫기
    		imageView.setOnMouseClicked( event -> popup.hide() );
    		
        // Label에 메세지 설정
        Label lblMessage = (Label) hbox.lookup("#lblMessage");
    		lblMessage.setText("메세지가 왔습니다.");
    		
    		popup.getContent().add(hbox);
    
        // 다른 윈도우로 포커스를 이동하면(마우스를 클릭하면)
        // 자동 닫힘 설정
    		popup.setAutoHide(true);
    		popup.show(primaryStage); // popup 띄우기
    	}
    }
    ```
    
    - ImageView를 마우스로 클릭하면 팝업이 닫히도록 했다.
        - 마우스 클릭 이벤트를 처리하려면 ImageView 객체가 필요
        - FXML 파일에서 로딩된 Parent의 `lookup()` 메소드를 이용해서 ImageView를 찾았다.
        - `lookup()` 메소드는 id속성값으로 내부 객체를 찾아올 때 유용하게 사용
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
    		FXMLLoader loader = new FXMLLoader(getClass().getResource("Root.fxml"));
    		Parent root = loader.load();
    		RootController controller = loader.getController();
    		controller.setPrimaryStage(primaryStage);
    		
    		Scene scene = new Scene(root);
    		
    		primaryStage.setTitle("AppMain");
    		primaryStage.setScene(scene);
    		primaryStage.show();
    	}
    
    	public static void main(String[] args) {
    		launch(args);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/Popup/Untitled.png)
    
    ![Untitled](/images/lang_java/javaFx/Popup/Untitled%201.png)
    
- Popup도 다이얼로그이므로 소유자 윈도우가 필요
    - `show()` 메소드를 호출할 때 매개값으로 소유가 윈도우 Stage가 필요

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판