---
title: "[Java] NIO, FileChannel 생성과 닫기"
description: ""
date: "2023-01-27T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"

---
<!--more-->

- `java.nio.channels.FileChannel`을 이용하면 파일 읽기와 쓰기를 할 수 있다.
- FileChannel은 동기화 처리가 되어 있기 때문에 멀티 스레드 환경에서 사용해도 안전
    
    ![Untitled](/images/lang_java/NIO/FileChannel_생성과_닫기/Untitled.png)
    
- FileChannel은 정적 메소드인 `open()`을 호출해서 얻을 수 있다.
    - IO의 FileInputStream, FileOutputStream의 `getChannel()` 메소드를 호출해서 얻을 수도 있다.
    - `open()` 메소드로 FileChannel을 얻는 방법
    
    ```java
    FileChannel fileChannel = FileChannel.open(Path path, OpenOption... options);
    ```
    
    - 첫 번째 path 매개값은 열거나, 생성하고자 하는 파일의 경로를 Path 객체로 생성해서 지정
    - 두 번째 options 매개값은 열기 옵션 값
        - StandardOpenOption의 열거 상수를 나열해주면 된다.
        
        | 열거 상수 | 설명 |
        | --- | --- |
        | READ | 읽기용으로 파일을 연다. |
        | WRITE | 쓰기용으로 파일을 연다. |
        | CREATE | 파일이 없다면 새 파일을 생성 |
        | CREATE_NEW | 새 파일을 생성. 이미 파일이 있으면 예외와 함께 실패 |
        | APPEND | 파일 끝에 데이터를 추가(WRITE나 CREATE와 함께 사용됨) |
        | DELETE_ON_CLOSE | 채널을 닫을 때 파일을 삭제(임시 파일을 삭제할 때 사용) |
        | TRUNCATE_EXISTING | 파일을 0바이트로 잘라낸다.(WRITE 옵션과 함께 사용됨) |
- ex) “C:\Temp\file.txt” 파일을 생성하고 어떤 내용을 쓰고 싶다면 매개값을 지정
    
    ```java
    FileChannel fileChannel = FileChannel.open(
    	Paths.get("C:/Temp/file.txt"),
    	StandardOpenOption.CREATE_NEW,
    	StandardOpenOption.WRITE
    );
    ```
    
- ex) “C:\Temp\file.txt” 파일을 읽고, 쓸 수 있도록 FileChannel을 생성
    
    ```java
    FileChannel fileChannel = FileChannel.open(
    	Paths.get("C:/Temp/file.txt"),
    	StandardOpenOption.READ,
    	StandardOpenOption.WRITE
    );
    ```
    
- FileChannel을 더 이상 이용하지 않을 경우에는 `close()` 메소드를 호출해서 닫아주어야 한다.
    
    ```java
    fileChannel.close();
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판