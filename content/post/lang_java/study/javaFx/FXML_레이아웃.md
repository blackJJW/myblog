---
title: "[Java] JavaFX, FXML 레이아웃"
description: ""
date: "2022-12-13T20:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- FXML은 XML 기반의 마크업 언어
    - JavaFX 애플리케이션의 UI 레이아웃을 자바 코드에서 분리해서 태그로 선언하는 방법을 제공
    - 이 방법은 안드로이드(Android) 앱을 개발하는 방법과 유사
        - 안드로이드는 XML 레이아웃을 작성하고, 자바로 이벤트 처리 및 애플리케이션 로직을 작성
    
- FXML 태그로 레이아웃을 정의하기 때문에 태그에 익숙한 디자이너와 협업이 가능
- 개발 완료 후 간단한 레이아웃 변경이나 스타일 변경이 필요할 경우에는 자바 소스를 수정할 필요없이 FXML 태그만 수정하면 된다.
    - 레이아웃이 비슷한 장면(Scene)들 간에 재사용이 가능하기 때문에 개발 기간을 단축시킬 수도 있다.
- ex) FXML 레이아웃
    - Root.fxml
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        
        <?import javafx.scene.layout.HBox?>
        <?import javafx.geometry.Insets?>
        <?import javafx.scene.control.*?>
        
        <HBox xmlns:fx="http://javafx.com/fxml/1"> <!-- HBox 컨테이너 선언 -->
        	<padding>                              <!-- 안쪽 여백 설정 -->
        		<Insets top="10" right="10" bottom="10" left="10"/>
        	</padding>
        	<spacing>10</spacing>                  <!-- 컨트롤 간의 수평 간격 설정 -->
        		
        	<children>                             <!-- 자식 컨트롤 추가 -->
        		<TextField>                        <!-- TextField 선언 -->
        			<prefWidth>200</prefWidth>     <!-- TextField의 폭 설정 -->
        		</TextField>
        			
        		<Button>                           <!-- Button 컨트롤 설정 -->
        			<text>확인</text>               <!-- Button 글자 설정 -->
        		</Button>
        	</children>
        </HBox>
        ```
        
        - 루트 컨테이너인 HBox는 `<HBox>` 태그로 작성
        - fx 접두사에 대한 네임스페이스 선언 `(xmlns:fx="http://javafx.com/fxml/1”)`이 추가
            - 이것은 FXML 파일에 `<fx:XXX>` 형태의 태그 및 `fx:xxx=”값"`형태의 속성을 작성할 수 있다는 의미
            - 이러한 태그와 속성은 컨트롤 배치보다는 FXML 파일의 내부 처리에 사용
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
        		Parent parent = FXMLLoader.load(getClass().getResource("Root.fxml"));
        		Scene scene = new Scene(parent);
        		
        		primaryStage.setTitle("Main");
        		primaryStage.setScene(scene);
        		primaryStage.show();
        	}
        	
        	public static void main(String[] args) {
        		launch(args);
        	}
        }
        ```
        
        ![Untitled](/images/lang_java/javaFx/FXML_레이아웃/Untitled.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판