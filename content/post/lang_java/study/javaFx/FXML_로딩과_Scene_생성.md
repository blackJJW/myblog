---
title: "[Java] JavaFX, FXML 로딩과 Scene 생성"
description: ""
date: "2022-12-14T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- FXML 파일을 작성했다면, FXML 파일을 읽어드려 선언된 내용을 객체화해야 한다.
    - 이것을 FXML 로딩이라고 한다.
- FXML 파일을 로딩하기 위해서는 `javafx.fxml.FXMLLoader`를 사용해야 한다.
    - FXMLLoder는 두 가지 종류의 로드 메소드를 가지고 있다.
    1. 정적 메소드인 `load()`
    2. 인스턴스 메소드인 `load()`
- FXML 파일이 클래스와 동일한 패키지에 있을 경우 정적 `load()` 메소드로 FXML 파일을 로딩하는 코드
    
    ```java
    Parent root = FXMLLoader.load(getClass().getResource("xxx.fxml"));
    ```
    
    - `getClass()`는 현재 클래스를 리턴
        - `getResource()`는 클래스가 위치하는 곳에서 상대 경로로 리소스의 URL을 리턴
- 인스턴스 `load()` 메소드로 FXML 파일을 로딩하는 코드
    
    ```java
    FXMLLoader loader = new FXMLLoader(getClass().getResource("xxx.fxml"));
    Parent root = (Parent)loader.load();
    ```
    
    - `load()` 메소드가 리턴하는 타입은 Parent 타입
    - 실제 객체는 FXML 파일에서 루트 태그로 선언된 컨테이너
    - FXML 파일에서 루트 태그가 `<HBox>`라면 다음과 같이 타입 변환이 가능
        
        ```java
        HBox hbox = (HBox) FXMLLoader.load(getClass().getResource("xxx.fxml"));
        ```
        
- FXML 파일을 로딩해서 Parent 객체를 얻었다면 이것을 가지고 다음과 같이 장면(Scene) 객체를 생성
    
    ```java
    Scene scene = new Scene(root);
    ```
    
- ex) FXML 로딩
    - Main.java
    
    ```java
    import javafx.application.Application;
    import javafx.fxml.FXMLLoader;
    import javafx.scene.Parent;
    import javafx.scene.Scene;
    import javafx.stage.Stage;
    
    public class Main extends Application {
    	@Override
    	public void start(Stage primaryStage) throws Exception {
    		Parent root = FXMLLoader.load(getClass().getResource("Root.fxml"));
    		Scene scene = new Scene(root);
    		
    		primaryStage.setTitle("Main");
    		primaryStage.setScene(scene);
    		primaryStage.show();
    	}
    	
    	public static void main(String[] args) {
    		launch(args);
    	}
    }
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판