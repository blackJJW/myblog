---
title: "[Java] JavaFX 다이얼로그, 컨트롤러에서 primaryStage 사용"
description: ""
date: "2022-12-26T14:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- 컨트롤러에서 다이얼로그를 실행할 때는 소유자 윈도우가 될 primaryStage가 필요
- 컨트롤러에서 primaryStage를 얻는 방법은 두 가지가 있다.

## 1. 메인 클래스에서 전달하는 방법

- primaryStage는 메인 클래스의 `start()` 매개값으로 전달되기 때문에 `start()` 메소드에서 컨트롤러로 primaryStage를 전달하면 된다.
- FXML 루트 태그의 `fx:controller` 속성에 지정된 컨트롤러 클래스는 FXMLLoader가 FXML을 로딩할 때 객체로 생성
    - FXMLLoader는 생성된 컨트롤러를 리턴하는 `getController()` 메소드를 제공
        - 그러나 이 메소드는 인스턴스 메소드이기 때문에 FXMLLoader 객체가 필요
        - FxMLLoader의 정적 메소드 `load()` 호출 코드는 인스턴스 메소드 `load()` 호출 코드로 변경해야 한다.
    
    ```java
    FXMLLoader loader = 
         new FXMLLoader(getClass().getResource("Root.fxml"));
    Parent root = loader.load();
    
    // Controller 객체를 얻어 primaryStage 전달
    RootController controller = loader.getController();
    controller.setPrimaryStage(primaryStage);
    ```
    
    - 마지막 코드를 보면 컨트롤러의 `setPrimaryStage()` 메소드를 호출하면서 primaryStage를 매개값으로 전달
        - 컨트롤러 클래스는 필드와 Setter 메소드를 추가해 두어야 한다.
    
    ```java
    public class RootController implements Initializable {
    	private Stage primaryStage;
    	public void setPrimaryStage(Stage primaryStage) {
    		this.primaryStage = primaryStage;
    	}
    	...
    }
    ```
    
- ex) 수정된 메인 클래스
    - AppMain.java
    
    ```java
    public class AppMain extends Application {
    	@Override
    	public void start(Stage primaryStage) throws Exception {
    		FXMLLoader loader = 
    				new FXMLLoader(getClass().getResource("Root.fxml"));
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
    

## 2. 컨테이너 또는 컨트롤로부터 얻는 방법

- 컨테이너나 컨트롤의 `getScene()` 메소드는 자신이 포함된 Scene 객체를 리턴
- Scene의 `getWindow()` 메소드는 자신을 보여주는 Stage 객체를 리턴
    - 다음과 같은 코드를 이용하면 컨트롤러에서 primaryStage를 얻을 수 있다.
    
    ```java
    Stage primaryStage = (Stage) 컨트롤.getScene().getWindow();
    ```
    
    - 주의할 점은 위 코드는 `initialize()` 메소드 안에서는 사용 불가
        - 아직 primaryStage가 생성되지 않았기 때문
        - 메인 클래스의 `start()` 메소드에서 Scene 객체를 생성하고, primaryStage에 Scene을 설정해야만 위 코드가 정상적으로 동작
        - 이벤트 처리 메소드 내에서 위 코드를 사용해야만 한다.
    - ex) Button을 클릭했을 때 실행하는 메소드가 `handleButton()`이라면 `handleButton()` 메소드에서 primaryStage를 얻어야 한다.
    
    ```java
    public class RootController implements Initializable {
    	@FXML private Button button;
    
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    	}
    	
    	public void handleButton(ActionEvent e) {
    		Stage primaryStage = (Stage) button.getScene().getWindow();
    		...
    	}
    }
    ```
    
    ---
    
- ex) 다섯 개의 버튼을 배치하고, 다이얼로그를 실행하도록 함
    - 다이얼로그
        - 파일 오픈 다이얼로그
        - 파일 저장 다이얼로그
        - 디렉토리 선택 다이얼로그
        - 팝업
        - 커스텀 다이얼로그
    - 선언적 레이아웃
        - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.layout.HBox?>
    
    <HBox spacing="10.0" xmlns="http://javafx.com/javafx/19" 
          xmlns:fx="http://javafx.com/fxml/1" 
          fx:controller="dialog.RootController">
       <children>
          <Button onAction="#handleOpenFileChooser" 
                  mnemonicParsing="false" text="Open FileChooser" />
          <Button onAction="#handleSaveFileChooser" 
                  mnemonicParsing="false" text="Save FileChooser" />
          <Button onAction="#handleDirectoryChooser" 
                  mnemonicParsing="false" text="DirectoryChooser" />
          <Button fx:id="btnPopup" onAction="#handlePopup" 
                  mnemonicParsing="false" text="Popup" />
          <Button fx:id="btnCustom" mnemonicParsing="false" 
                  onAction="#handleCustom" text="Custom" />
       </children>
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
    </HBox>
    ```
    
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
    
    - Custom_dialog.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.Label?>
    <?import javafx.scene.image.Image?>
    <?import javafx.scene.image.ImageView?>
    <?import javafx.scene.layout.AnchorPane?>
    
    <AnchorPane prefHeight="150.0" prefWidth="400.0" 
    			xmlns:fx="http://javafx.com/fxml/1" 
    			xmlns="http://javafx.com/javafx/19">
       <children>
          <ImageView fitHeight="50.0" fitWidth="50.0" 
          			 layoutX="15.0" layoutY="15.0" 
          			 pickOnBounds="true" preserveRatio="true">
             <image>
                <Image url="@images/dialog-info.png" />
             </image>
          </ImageView>
          <Button id="btnOk" layoutX="336.0" layoutY="104.0" 
          		  mnemonicParsing="false" text="확인" />
          <Label id="txtTitle" layoutX="87.0" layoutY="33.0" 
          		 prefHeight="15.0" prefWidth="290.0" />
       </children>
    </AnchorPane>
    ```
    
    - 컨트롤러
        - RootController.java
    
    ```java
    import java.io.File;
    import java.net.URL;
    import java.util.ResourceBundle;
    
    import javafx.event.ActionEvent;
    import javafx.fxml.FXMLLoader;
    import javafx.fxml.Initializable;
    import javafx.scene.Scene;
    import javafx.scene.control.Button;
    import javafx.scene.control.Label;
    import javafx.scene.image.Image;
    import javafx.scene.image.ImageView;
    import javafx.scene.layout.AnchorPane;
    import javafx.scene.layout.HBox;
    import javafx.stage.DirectoryChooser;
    import javafx.stage.FileChooser;
    import javafx.stage.FileChooser.ExtensionFilter;
    import javafx.stage.Modality;
    import javafx.stage.Popup;
    import javafx.stage.Stage;
    import javafx.stage.StageStyle;
    
    public class RootController implements Initializable {
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    	}
    	
    	private Stage primaryStage;
    	public void setPrimaryStage(Stage primaryStage) {
    		this.primaryStage = primaryStage;
    	}
    	
    	// 파일 열기 다이얼로그 실행
    	public void handleOpenFileChooser(ActionEvent e) {
    		FileChooser fileChooser = new FileChooser();
    		fileChooser.getExtensionFilters().addAll(
    			new ExtensionFilter("Text Files", "*.txt"),
    			new ExtensionFilter("Image Files", "*.png", "*.jpg", "*.gif"),
    			new ExtensionFilter("Audio Files", "*.wav", "*.mp3", "*.acc"),
    			new ExtensionFilter("All Files", "*.*")	
    		);
    		File selectedFile = fileChooser.showOpenDialog(primaryStage);
    		if(selectedFile != null) {
    			System.out.println(selectedFile.getPath());
    		}
    	}
    	
    	// 파일 저장 다이얼로그 실행
    	public void handleSaveFileChooser(ActionEvent e) {
    		FileChooser fileChooser = new FileChooser();
    		fileChooser.getExtensionFilters().add(new ExtensionFilter("All Files", "*.*"));
    		File selectedFile = fileChooser.showSaveDialog(primaryStage);
    		if(selectedFile != null) {
    			System.out.println(selectedFile.getPath());
    		}
    	}
    	
    	// 디렉토리 선택 다이얼로그 실행
    	public void handleDirectoryChooser(ActionEvent e) {
    		DirectoryChooser directoryChooser = new DirectoryChooser();
    		File selectedDir = directoryChooser.showDialog(primaryStage);
    		if(selectedDir != null) {
    			System.out.println(selectedDir.getPath());
    		}
    	}
    	
    	// Popup 다이얼로그 실행
    	public void handlePopup(ActionEvent e) throws Exception {
    		Popup popup = new Popup();
    		
    		HBox hbox = (HBox) FXMLLoader.load(getClass().getResource("Popup.fxml"));
    		ImageView imageView = (ImageView) hbox.lookup("#imgMessage");
    		imageView.setImage(
    			new Image(getClass().getResource("images/dialog-info.png").toString())
    		);
    		imageView.setOnMouseClicked( event -> popup.hide() );
    		Label lblMessage = (Label) hbox.lookup("#lblMessage");
    		lblMessage.setText("메세지가 왔습니다.");
    		
    		popup.getContent().add(hbox);
    		popup.setAutoHide(true);
    		popup.show(primaryStage);
    	}
    	
    	// 커스텀 다이얼로그 실행
    	public void handleCustom(ActionEvent e) throws Exception {
    		Stage dialog = new Stage(StageStyle.UTILITY);
    		
    		dialog.initModality(Modality.WINDOW_MODAL);
    		dialog.initOwner(primaryStage);
    		dialog.setTitle("확인"); // 다이얼로그 제목
    		
    		AnchorPane anchorPane = 
    				(AnchorPane) FXMLLoader.load(getClass().getResource("Custom_dialog.fxml"));
    		
    		Label txtTitle = (Label) anchorPane.lookup("#txtTitle"); // 메시지 설정
    		txtTitle.setText("확인하셨습니까?");
    		
    		// 버튼 이벤트 처리
    		Button btnOk = (Button) anchorPane.lookup("#btnOk");
    		btnOk.setOnAction( event -> dialog.close() );
    		
    		Scene scene = new Scene(anchorPane); // Scene 생성
    		
    		dialog.setScene(scene); // 다이얼로그에 Scene 설정
    		dialog.setResizable(false); // 크기를 변경하지 못하도록 설정
    		dialog.show();  // 다이얼로그 띄우기
    	}
    }
    ```
    
    - 메인 클래스
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
    
    ![Untitled](/images/lang_java/javaFx/컨트롤러에서_primaryStage_사용/Untitled.png)
    
    ![Untitled](/images/lang_java/javaFx/컨트롤러에서_primaryStage_사용/Untitled%201.png)
    
    ![Untitled](/images/lang_java/javaFx/컨트롤러에서_primaryStage_사용/Untitled%202.png)
    
    ![Untitled](/images/lang_java/javaFx/컨트롤러에서_primaryStage_사용/Untitled%203.png)
    
    ![Untitled](/images/lang_java/javaFx/컨트롤러에서_primaryStage_사용/Untitled%204.png)
    
    ![Untitled](/images/lang_java/javaFx/컨트롤러에서_primaryStage_사용/Untitled%205.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판