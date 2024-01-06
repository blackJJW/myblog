---
title: "[Java] JavaFX, 메뉴바와 툴바"
description: ""
date: "2022-12-25T19:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- JavaFX도 메뉴바와 툴바를 생성할 수 있도록 MenuBar 컨트롤과 ToolBar 컨트롤을 제공

## 1. MenuBar 컨트롤

![Untitled](/images/lang_java/javaFx/JavaFX_메뉴바와_툴바/Untitled.png)

- MenuBar 컨트롤은 컨테이선 상단에 배치
    - 다양한 작업을 쉽게 선택하도록 해준다.
- MenuBar에는 메뉴 아이템으로 MenuItem, CheckMenuItem, RadioMenuItem, CustomMenuItem, SeparatorMenuItem을 추가 가능
    - 서브 메뉴를 갖는 Menu도 추가 가능
    - 위 그림에서 “Shuffle”, “Clear”, “Exit”가 MenuItem에 해당
    - 가로 구분선은 SeparatorMenuItem
    - CheckMenuItem, RadioMenuItem은 두 가지 상태를 가지는 메뉴 아이템
        - CheckMenuItem을 클릭하면 선택, 다시 클릭하면 미 선택이 된다.
        - ToggleGroup을 참조하는 RadioMenuItem들은 하나를 클릭하면 다른 것들이 선택 해제
- ex) FXML로 MenuBar 컨트롤을 선언하는 방법
    
    ```xml
    <MenuBar>
    	<menus>
    		<Menu text="파일"> ... </Menu>
    		<Menu text="편집"> ... </Menu>
    	</menus>
    </MenuBar>
    ```
    
- ex) “파일” 메뉴의 메뉴 아이템을 추가하는 방법
    
    ```xml
    <Menu text="파일">
    	<items>
    		<MenuItem text="새로만들기" onAction="#handleNew">
    			<accelerator>
    				<KeyCodeCombination alt="DOWN" code="N" 
          control="UP" meta="UP" shift="DOWN" shrotcut="UP" />
    			</accelerator>
    			<graphic>
    				<ImageView>
    					<image>
    						<Image url="@icons/new.png" />
               </image>
    				</ImageView>
    			</graphic>
    		</MenuItem>
    	</items>
    </Menu>
    ```
    
    - MenuItem도 Button과 마찬가지로 클릭하면 onAction 속성에 지정된 컨트롤러의 메소드를 호출해서 ActionEvent를 처리
    - `<accerlerator>`는 단축키를 설정
        - 단축키는 KeyCodeCombination 객체로 생성
        - `Alt`키, `Control`키, `Shift`키, code키의 조합으로 구성 가능
            - DOWN으로 설정된 키와 code키를 동시에 누르면 onAction 속성에 지정된 메소드가 호출
    - “새로만들기” 메뉴 아이템일 경우 `Alt`+`Shift`+`N`을 동시에 누르면 `handleNew()` 메소드가 실행
    - `<graphic>`는 메뉴 아이템 앞에 아이콘을 추가

## 2. ToolBar 컨트롤

- 계층적인 작업 선택 기능은 MenuBar 컨트롤이 편리하나, 빠르게 작업을 선택하고 싶을 경우 Toolbar 컨트롤을 추가하는 것이 좋다.
- Toolbar 컨트롤은 UI 컨트롤이면서 컨테이너이기도 하다.
    - 주로 Button이 추가
    - ComboBox와 같은 다른 컨트롤도 배치 가능
- ex) FXML로 Toolbar를 선언하는 방법
    
    ```xml
    <ToolBar>
    	<items>
    		<Button onAction="#handleNew">
    			<graphic>
    				<ImageView>
    					<image>
    						<Image url="@icons/new.png" />
    					</image>
    				</ImageView>
    			</graphic>
    		</Button>
    	</items>
    </ToolBar>
    ```
    
- ex) 상단에 MenuBar와 ToolBar를 추가하고 중앙에는 TextArea를 추가해서 간단한 메모장을 구현
    - MenuBar의 메뉴 아이템과 ToolBar의 버튼을 클릭하면 TextArea에 선택된 작업 내용이 출력
    - Root.fxml
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <?import java.lang.String?>
    <?import javafx.collections.FXCollections?>
    <?import javafx.scene.control.Button?>
    <?import javafx.scene.control.ComboBox?>
    <?import javafx.scene.control.Menu?>
    <?import javafx.scene.control.MenuBar?>
    <?import javafx.scene.control.MenuItem?>
    <?import javafx.scene.control.SeparatorMenuItem?>
    <?import javafx.scene.control.TextArea?>
    <?import javafx.scene.control.ToolBar?>
    <?import javafx.scene.image.Image?>
    <?import javafx.scene.image.ImageView?>
    <?import javafx.scene.input.KeyCodeCombination?>
    <?import javafx.scene.layout.BorderPane?>
    <?import javafx.scene.layout.VBox?>
    
    <BorderPane prefHeight="200.0" prefWidth="400.0" xmlns="http://javafx.com/javafx/19" xmlns:fx="http://javafx.com/fxml/1" fx:controller="menuBarToolBar.RootController">
       <top>
          <VBox BorderPane.alignment="CENTER">
             <children>
                <MenuBar>
                  <menus>
                      <Menu mnemonicParsing="false" text="파일">
                        <items>
                            <MenuItem mnemonicParsing="false" onAction="#handleNew" text="새로만들기">
                               <accelerator>
                               	<KeyCodeCombination alt="DOWN" code="N" control="UP" meta="UP" shift="DOWN" shortcut="UP" />
                               </accelerator>
                               <graphic>
                                  <ImageView fitHeight="15.0" fitWidth="20.0" pickOnBounds="true" preserveRatio="true">
                                     <image>
                                        <Image url="@icons/new.png" />
                                     </image>
                                  </ImageView>
                               </graphic>
                            </MenuItem>
                            <MenuItem mnemonicParsing="false" onAction="#handleOpen" text="열기">
                               <accelerator>
                               	<KeyCodeCombination alt="UP" code="O" control="DOWN" meta="UP" shift="UP" shortcut="UP" />
                               </accelerator>
                               <graphic>
                                  <ImageView fitHeight="15.0" fitWidth="20.0" pickOnBounds="true" preserveRatio="true">
                                     <image>
                                        <Image url="@icons/open.png" />
                                     </image>
                                  </ImageView>
                               </graphic>
                            </MenuItem>
                            <MenuItem mnemonicParsing="false" onAction="#handleSave" text="저장">
                               <graphic>
                                  <ImageView fitHeight="15.0" fitWidth="20.0" pickOnBounds="true" preserveRatio="true">
                                     <image>
                                        <Image url="@icons/save.png" />
                                     </image>
                                  </ImageView>
                               </graphic>
                            </MenuItem>
                            <SeparatorMenuItem mnemonicParsing="false" />
                            <MenuItem mnemonicParsing="false" onAction="#handleExit" text="끝내기" />
                        </items>
                      </Menu>
                  </menus>
                </MenuBar>
            
                <ToolBar prefHeight="40.0" prefWidth="200.0">
                  <items>
                    <Button mnemonicParsing="false" onAction="#handleNew">
                         <graphic>
                            <ImageView fitHeight="15.0" fitWidth="20.0" pickOnBounds="true" preserveRatio="true">
                               <image>
                                  <Image url="@icons/new.png" />
                               </image>
                            </ImageView>
                         </graphic>
                      </Button>
                      <Button mnemonicParsing="false" onAction="#handleOpen">
                         <graphic>
                            <ImageView fitHeight="15.0" fitWidth="20.0" pickOnBounds="true" preserveRatio="true">
                               <image>
                                  <Image url="@icons/open.png" />
                               </image>
                            </ImageView>
                         </graphic>
                      </Button>
                      <Button mnemonicParsing="false" onAction="#handleSave">
                         <graphic>
                            <ImageView fitHeight="15.0" fitWidth="20.0" pickOnBounds="true" preserveRatio="true">
                               <image>
                                  <Image url="@icons/save.png" />
                               </image>
                            </ImageView>
                         </graphic>
                      </Button>
                      <ComboBox prefWidth="100.0" promptText="선택">
                      	<items>
                      		<FXCollections fx:factory="observableArrayList">
                      			<String fx:value="공개" />
                      			<String fx:value="비공개" />
                      		</FXCollections>
                      	</items>
                      </ComboBox>
                  </items>
                </ToolBar>
             </children>
          </VBox>
       </top>
       <center>
          <TextArea fx:id="textArea" prefHeight="200.0" prefWidth="200.0" BorderPane.alignment="CENTER" />
       </center>
    </BorderPane>
    ```
    
    - ActionEvent 처리
        - RootController.java
    
    ```java
    import java.net.URL;
    import java.util.ResourceBundle;
    
    import javafx.application.Platform;
    import javafx.event.ActionEvent;
    import javafx.fxml.FXML;
    import javafx.fxml.Initializable;
    import javafx.scene.control.TextArea;
    
    public class RootController implements Initializable {
    	@FXML private TextArea textArea;
    	
    	@Override
    	public void initialize(URL location, ResourceBundle resources) {
    	}
    	
    	public void handleNew(ActionEvent e) {
    		textArea.appendText("New\n");
    	}
    	
    	public void handleOpen(ActionEvent e) {
    		textArea.appendText("Open\n");
    	}
    	
    	public void handleSave(ActionEvent e) {
    		textArea.appendText("Save\n");
    	}
    	
    	public void handleExit(ActionEvent e) {
    		Platform.exit();
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_메뉴바와_툴바/Untitled%201.png)
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_메뉴바와_툴바/Untitled%202.png)
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_메뉴바와_툴바/Untitled%203.png)
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_메뉴바와_툴바/Untitled%204.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판
- [https://docs.oracle.com/javafx](https://docs.oracle.com/javafx)