---
title: "[Java] NIO, 와치 서비스(WatchService)"
description: ""
date: "2023-01-23T17:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
---
<!--more-->

- 디렉토리 내부에서 파일 생성, 삭제 수정 등의 내용 변화를 감시하는 데 사용
- 와치 서비스는 일반적으로 파일 변경 통지 매커니즘
- WatchService를 생성하려면 FileSystem의 `newWatchService()` 메소드를 호출
    
    ```java
    WatchService watchService 
    		= FileSystem.getDefault().newWatchService();
    ```
    
    - WatchService를 생성했다면 감시가 필요한 디렉토리의 Path 객체에서 `register()` 메소드로 WatchService를 등록
        - 이때 어떤 변화(생성, 삭제, 수정)를 감시할 것인지를 StandardWatchEventKinds 상수로 지정 가능
    - 생성, 수정, 삭제를 감시하도록 WatchService를 등록
    
    ```java
    path.register(watchService, 
    			StandardWatchEventKinds.ENTRY_CREATE,
    			StandardWatchEventKinds.ENTRY_MODIFY,
    			StandardWatchEventKinds.ENTRY_DELETE);
    ```
    
- 디렉토리(Path)에 WatchService를 등록한 순간부터 디렉토리 내부에서 변경이 발생되면 와치 이벤트(WatchEvent)가 발생
    - WatchService는 해당 이벤트 정보를 가진 WatchKey를 생성하여 큐(Queue)에 넣어준다.
- 프로그램은 무한 루프를 돌면서 WatchService의 take() 메소드를 호출하여 WatchKey가 큐에 들어올 때까지 대기
    - WatchKey가 큐에 들어오면 WatchKey를 얻어 처리
    
    ```java
    while(true){
    		// 큐에 WatchKey가 돌어올 때까지 대기
    		WatchKey watchKey = watchService.take();
    }
    ```
    
- WatchKey를 얻고나서 해야 할 일은 `pollEvents()` 메소드를 호출해서 WatchEvent 리스트를 얻어내는 것
    - 한 개의 WatchEvent가 아니라 `List<WatchEvent<?>>`로 리턴하는 이유
        - 여러 개의 파일이 동시에 삭제, 수정, 생성될 수 있기 때문
        - WatchEvent는 파일당 하나씩 발생
    
    ```java
    List<WatchEvent<?>> list = watchKey.pollEvents();
    ```
    
    - 프로그램은 WatchEvent 리스트에서 WatchEvent를 하나씩 꺼내어 이벤트의 종류와 Path 객체를 얻어낸 다음 적절히 처리
    
    ```java
    for(WatchEvent watchEvent : list) {
    	// 이벤트 종류 얻기
    	Kind kind = watchEvent.kind();
    	// 감지된 Path 얻기
    	Path path = (Path)watchEvent.context();
    	// 이벤트 종류별로 처리
    	if(kind == StandardWatchEventKinds.ENTRY_CREATE) {
    		// 생성되었을 경우, 실행할 코드
    	} else if(kind == StandardWatchEventKinds.ENTRY_DELETE) {
    		// 삭제되었을 경우, 실행할 코드
    	} else if(kind == StandardWatchEventKinds.ENTRY_MODIFY) {
    		// 변경되었을 경우, 실행할 코드
    	} else if(kind == StandardWatchEventKinds.OVERFLOW) {
    	}
    }
    ```
    
    - OVERFLOW 이벤트는 운영체제에서 이벤트가 소실됐거나 버려진 경우에 발생
        - 별도의 처리 코드가 필요없다.
        - CREATE, DELETE, MODIFY 이벤트만 처리하면 된다.
- 한 번 사용된 WatchKey는 `reset()` 메소드로 초기화
    - 새로운 WatchEvent가 발생하면 큐에 다시 들어가기 때문
    - 초기화에 성공하면 `reset()` 메소드는 true를 리턴
    - 감시하는 디렉토리가 삭제되었거아 키가 더 이상 유효하지 않을 경우 false를 리턴
    - WatchKey가 더 이상 유효하지 않게 되면 무한 루프를 빠져나와 WatchService의 `close()` 메소드를 호출하고 종료
    
    ```java
    while(true) {
    	WatchKey watchKey = watchService.take();
    	List<WatchEvent<?>> list = watchKey.pollEvents();
    	
    	for(WatchEvent watchEvent : list) {
    		...
    	}
    	boolean valid = watchKey.reset();
    	if(!valid) { break; }
    }
    watchService.close();
    ```
    
- ex) C:\Temp 디렉토리를 감시 디렉토리로 설정
    - 실행 후
        - C:\Temp\dir 디렉토리와 C:\Temp\file.txt 파일을 생성
        - file.txt 파일 내용을 수정한 다음 저장
        - dir, file.txt를 동시에 삭제
    - 이 행위들이 TextArea에 기록된다.
    - WatchServiceEx.java
    
    ```java
    import java.nio.file.FileSystems;
    import java.nio.file.Path;
    import java.nio.file.Paths;
    import java.nio.file.StandardWatchEventKinds;
    import java.nio.file.WatchEvent;
    import java.nio.file.WatchEvent.Kind;
    import java.nio.file.WatchKey;
    import java.nio.file.WatchService;
    import java.util.List;
    
    import javafx.application.Application;
    import javafx.application.Platform;
    import javafx.scene.Scene;
    import javafx.scene.control.TextArea;
    import javafx.scene.layout.BorderPane;
    import javafx.stage.Stage;
    
    public class WatchServiceEx extends Application {
    	// WatchServiceThread 클래스 선언
    	class WatchServiceThread extends Thread {
    		@Override
    		public void run() {
    			try {
    				// C:\Temp 디렉토리에 WatchService 등록
    				WatchService watchService 
    					= FileSystems.getDefault().newWatchService();
    				Path directory = Paths.get("C:/Temp");
    				directory.register(watchService, 
    						StandardWatchEventKinds.ENTRY_CREATE,
    						StandardWatchEventKinds.ENTRY_DELETE,
    						StandardWatchEventKinds.ENTRY_MODIFY);
    				
    				while(true) {
    					// 블로킹(WatchKey가 큐에 들어올 때까지
    					WatchKey watchKey = watchService.take();
    					// WatchEvent 목록 얻기
    					List<WatchEvent<?>> list = watchKey.pollEvents();
    					for(WatchEvent watchEvent : list) {
    						// 이벤트 종료 얻기
    						Kind kind = watchEvent.kind();
    						// 감지된 Path 얻기
    						Path path = (Path)watchEvent.context();
    						if(kind == StandardWatchEventKinds.ENTRY_CREATE) {
    							// 생성되었을 경우, 실행할 코드
    							Platform.runLater(() -> textArea.appendText("파일 생성됨 -> "
    													+ path.getFileName() + "\n"));
    						} else if(kind == StandardWatchEventKinds.ENTRY_DELETE) {
    							// 삭제되었을 경우, 실행할 코드
    							Platform.runLater(() -> textArea.appendText("파일 삭제됨 -> "
    									+ path.getFileName() + "\n"));
    						} else if(kind == StandardWatchEventKinds.ENTRY_MODIFY) {
    							// 변경되었을 경우, 실행할 코드
    							Platform.runLater(() -> textArea.appendText("파일 변경됨 -> "
    									+ path.getFileName() + "\n"));
    						} else if(kind == StandardWatchEventKinds.OVERFLOW) {
    						}
    					}
    					boolean valid = watchKey.reset();
    					if(!valid) { break; }
    				}
    			} catch(Exception e) {}
    		}
    	}
    	
    	TextArea textArea;
    	
    	@Override
    	public void start(Stage primaryStage) throws Exception{
    		BorderPane root = new BorderPane();
    		root.setPrefSize(500, 300);
    		
    		textArea = new TextArea();
    		textArea.setEditable(false);
    		root.setCenter(textArea);
    		
    		Scene scene = new Scene(root);
    		primaryStage.setScene(scene);
    		primaryStage.setTitle("WatchServiceEx");
    		primaryStage.show();
    		
    		// WatchServiceThread 생성 및 시작
    		WatchServiceThread wst = new WatchServiceThread();
    		wst.start();
    	}
    	
    	public static void main(String[] args) {
    		launch(args);
    	}
    }
    ```
    
    ![2023-01-24 01 54 34](/images/lang_java/NIO/watchService/2023-01-24_01_54_34.gif)

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판