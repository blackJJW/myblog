---
title: "[Java] JavaFX, FXML 작성 규칙"
description: ""
date: "2022-12-14T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- FXML로 선언된 태그는 자바 코드로 변환되어 실행되기 때문에 자바 코드와 매핑 관계가 존재
    - 이 매핑 관계만 잘 이해하면 JavaFX API 도큐먼트를 참조해서 FXML 태그를  쉽게 작성할 수 있다.
    
    ### 프로그램적 레이아웃 자바 코드와 FXML 레이아웃 태그를 매핑시킨 표
    
    ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled.png)
    
- FXML은 XML기반의 마크업 언어이기 때문에 XML작성 규칙을 잘 지켜서 작성해야 한다.

## 1. 패키지 선언

- 자바 코드의 패키지 선언과 매핑되는 FXML 태그는 `<?import?>`이다.
- 클래스 하나를 import하는 방법과 같은 패키지의 모든 클래스를 import하는 방법은 다음과 같다.
    
    ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled%201.png)
    
- `<?import?>` 태그를 작성하는 위치는 정해져 있다.
    - XML 선언 태그인 `<?xml version=”1.0” encoding=”UTF-8”?>`과 루트 컨테이너 태그 사이
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import javafx.scene.layout.HBox?>
    <?import javafs.scene.control.*?>
    
    <루트컨테이너 xmlns:fx="http://javafx.com/fxml">
    
    ...
    
    </루트컨테이너>
    ```
    
    - FXML 태그의 이름은 하나의 JavaFX API 이름과 매핑되기 때문에 해당 클래스가 존재하는 패키지를 반드시 `<?import?>` 태그로 선언해야 한다.
        - 그렇지 않으면 FXML을 로딩할 때 not a valid type이라는 메시지와 함께 `javafx.fxml.LoadException`이 발생

## 2. 태그 선언

- FXML 태그는 `<`와 `>` 사이에 태그 이름을 작성
- 반드시 시작 태그가 있으면 끝 태그도 있어야 한다.
    - 그렇지 않으면 `javax.xml.XMLStreamException` 예외가 발생
    
    ```xml
    <태그이름> ... </태그이름>
    ```
    
- 시작 태그와 끝 태그 사이에는 태그 내용이 작성
    - 태그 내용이 없을 경우, 시작 태그 끝에 `/>`를 붙이고 끝 태그를 생략 가능
    
    ```xml
    <태그이름 />
    ```
    
- 태그 이름은 JavaFX의 클래스명이거나, Setter의 클래스명이 될 수 있다.
    
    ### Button 컨트롤, 자바 코드와 FXML 태그 비교
    
    ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled%202.png)
    

## 3. 속성 선언

- FXML 태그는 다음과 같은 속성을 가질 수 있다.
    - 속성 값은 큰 따옴표( ” ” ) 또는 작은 따옴표( ‘ ‘ )로 반드시 감싸야 한다.
        - 그렇지 않으면 `javax.xml.stream.XMLStreamException` 예외가 발생
    
    ```xml
    <태그이름 속성명="값" 속성명="값"> ... </태그이름>
    ```
    
- 속성명은 Setter 속성명이 온다.
    - 모든 Setter가 사용될 수 있는 것은 아니다.
    - 기본 타입(boolean, byte, short, char, int, long, float, double)의 값을 세팅하거나, String(문자열)을 세팅하는 Setter만 올 수 있다.
    - ex) Button의 글자를 설정 시, setText() 메소드를 사용
        - 매개값이 문자열이므로 다음과 같이 text 속성으로 작성 가능
        
        ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled%203.png)
        

## 4. 객체 선언

- Setter 메소드가 기본 타입과 String 타입이 아닌 다른 타입의 객체를 매개값으로 갖는 경우
    - 속성으로 작성 불가
    - 태그로 작성 가능
- 매개값인 객체를 태그로 선언하는 방법
    
    ### | <클래스 속성=”값”/> |
    
    - 일반적으로 다음과 같이 클래스명으로 태그를 작성하면 new 연산자로 기본 생성자를 호출해서 객체를 생성
        
        ```xml
        <클래스>
        ```
        
    - 만약 생성자에 매개 변수가 있고, 매개 변수에 `@NamedArg(javafx.beans.NamedArg)` 어노테이션이 적용되어 있다면 속성명이나 자식 태그로 작성 가능
        
        ```xml
        <클래스 매개변수="값">
        </클래스>
        ```
        
        ```xml
        <클래스>
        	<매개변수>값</매개변수>
        </클래스>
        ```
        
    - ex) HBox의 패딩을 설정할 때 `setPadding(Insets value)` 메소드를 사용
        - `Insets`는 기본 생성자가 없다.
            - `Insets(double topRightBottomLeft)` 또는 `Insets(double top, double right, double bottom, double left)` 만 있다.
        - 이 경우 `Insets` 객체를 FXML로 선언하면 다음과 같다.
        
        ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled%204.png)
        
    
    ### | <클래스 fx:value=”값”/> |
    
    - 클래스가 `valueOf(String)` 메소드를 제공해서 객체를 생성하는 경우가 있다.
    - ex) String, Integer, Double, Boolean 클래스는 `valueOf(String)`을 호출해서 객체를 생성
        - 이 경우 다음과 같이 FXML 태그를 작성할 수 있다.
        
        ```xml
        <클래스 fx:value="값" />
        ```
        
    - ex) String, Integer, Double, Boolean 객체를 FXML로 선언
        
        ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled%205.png)
        
    
    ### | <클래스 fx:constant=”상수”/> |
    
    - 클래스에 정의된 상수값을 얻고 싶을 경우 다음과 같이 FXML 태그를 작성
        
        ```xml
        <클래스 fx:constant="상수" />
        ```
        
    - ex) `Double.MAX_VALUE` 상수값을 Button 컨트롤의 maxWidth 속성값으로 설정할 경우 다음과 같이 FXML로 선언
        
        ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled%206.png)
        
    
    ### | <클래스 fx:factory=”정적메소드”> |
    
    - 어떤 클래스는 `new` 연산자로 객체를 생성 불가
        - 정적 메소드로 객체를 얻어야 하는 경우도 있다.
        - 이 경우 다음과 같이 FXML 태그를 작성
        
        ```xml
        <클래스 fx:factory="정적메소드">
        ```
        
    - ex) ComboBox의 `setItems(ObservableList<T> value)` 메소드는 `javafx.collections.ObservableList` 인터페이스 타입의 구현 객체를 매개값으로 가진다.
        - ObservableList의 구현 객체는 `javafx.collections.FXCollections`의 정적 메소드인 `observableArrayList(E … items)` 메소드로 얻을 수 있다.
        - 다음과 같이 FXML을 작성
        
        ![Untitled](/images/lang_java/javaFx/FXML_작성_규칙/Untitled%207.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판