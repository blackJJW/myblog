---
title: "[Java] JavaFX 다이얼로그, FileChooser, DirectoryChooser"
description: ""
date: "2022-12-25T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- FileChooser는 로컬 PC의 파일을 선택할 수 있는 다이얼로그
    - 열기 또는 저장 옵션으로 실행 가능
    - 파일 확장명을 필터링해서 원하는 파일만 볼 수 있다.
- FileChooser는 컨트롤이 아니기 때문에 FXML에서 선언 불가
    - 버튼이나 메뉴 아이템의 ActionEvent를 처리할 때 자바 코드로 FileChooser를 생성
    - `showOpenDialog()` 또는 `showSaveDialog()`를 호출
- ex) 열기 옵션으로 FileChooser를 실행한 코드
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.geometry.Insets?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.layout.HBox?>
    
    <HBox fx:controller="dialog.RootController" spacing="10.0" 
    	  xmlns:fx="http://javafx.com/fxml/1" 
    	  xmlns="http://javafx.com/javafx/19">
       <children>
          <Button onAction="#handleOpenFileChooser" 
          		  mnemonicParsing="false" text="Open FileChooser" />
       </children>
       <padding>
          <Insets bottom="10.0" left="10.0" right="10.0" top="10.0" />
       </padding>
    </HBox>
    ```
    
    - RootController.java
    
    ```java
    import java.io.File;
    import java.net.URL;
    import java.util.ResourceBundle;
    
    import javafx.event.ActionEvent;
    import javafx.fxml.Initializable;
    import javafx.stage.FileChooser;
    import javafx.stage.FileChooser.ExtensionFilter;
    import javafx.stage.Stage;
    
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
    
        // 다이얼로그 띄우기
    		File selectedFile = fileChooser.showOpenDialog(primaryStage);
    		if(selectedFile != null) {
          // 선택된 파일 경로 얻기
    			System.out.println(selectedFile.getPath());
    		}
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
    
    ![Untitled](/images/lang_java/javaFx/FileChooser,_DirectoryChooser/Untitled.png)
    
    ![Untitled](/images/lang_java/javaFx/FileChooser,_DirectoryChooser/Untitled%201.png)
    
    - 파일 확장명으로 파일 이름을 필터링하면 ExtensionFilter를 생성해서 추가하면 된다.
    - `showOpenDialog()` 또는 `showSaveDialog()`를 호출할 때에는 소유자 윈도우(Stage)를 매개값으로 제공해야 한다.
        - 그 이유는 다이얼로그는 자체적으로 실행할 수 없고, 소유가 윈도우가 있어야 하기 때문
- FileChooser는 모달 다이얼로그이므로 [열기] 또는 [저장] 버튼을 클릭하거나, [취소] 버튼을 클릭하기 전까지는 소유자 윈도우를 사용할 수 없다.
- 파일을 선택하고 [열기] 또는 [저장] 버튼을 클릭하면, `showOpenDialog()` 또는 `showSaveDialog()`가 File 객체를 리턴
- 선택된 파일의 전체 경로를 문자열로 얻고 싶다면 `getPath()`를 호출하면 된다.
    
    ![Untitled](/images/lang_java/javaFx/FileChooser,_DirectoryChooser/Untitled%202.png)
    
    ![Untitled](/images/lang_java/javaFx/FileChooser,_DirectoryChooser/Untitled%203.png)
    
    - 파일이 아니라 디렉토리를 선택하고 싶다면 FileChooser 대신 DirectoryChooser를 사용하면 된다.
        - 사용 방법은 FileChooser와 비슷
    
    ```java
    // 디렉토리 선택 다이얼로그 실행
    public void handleDirectoryChooser(ActionEvent e) {
    	DirectoryChooser directoryChooser = new DirectoryChooser();
    	File selectedDir = directoryChooser.showDialog(primaryStage);
    	if(selectedDir != null) {
    		System.out.println(selectedDir.getPath());
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/FileChooser,_DirectoryChooser/Untitled%204.png)
    
    ![Untitled](/images/lang_java/javaFx/FileChooser,_DirectoryChooser/Untitled%205.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판