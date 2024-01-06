---
title: "[Java] NIO, AsynchronousFileChannel 생성과 닫기"
description: ""
date: "2023-01-28T14:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"

---
<!--more-->

- AsynchronousFileChannel은 두 가지 정적 메소드인 `open()`을 호출해서 얻을 수 있다.
- 첫 번째 `open()` 메소드는 파일의 Path 객체와 열기 옵션 값을 매개값으로 받는다.
    
    ```java
    AsychronousFileChannel fileChannel 
    		= AsynchronousFileChannel.open(
    				Path file,
    				OpenOption... options
    			);
    ```
    
    - 이렇게 생성된 AsynchronousFileChannel은 내부적으로 생성되는 기본 스레드들을 이용해서 스레드를 관리
- 기존 스레드풀의 최대 스레드 개수는 개발자가 지정할 수 없다.
    - 두 번째 `open()` 메소드로 AsynchronousFileChannel을 만들 수 있다.
    
    ```java
    AsychronousFileChannel fileChannel 
    		= AsynchronousFileChannel.open(
    				Path file,
    				Set<? extends OpenOption> options,
    				ExecutorService executor,
    				FileAttribute<?>... attrs
    			);
    ```
    
    - file 매개값은 파일의 Path 객체
    - options 매개값은 열기 옵션 값들이 저장된 Set 객체
    - attrs 매개값은 파일 생성 시 파일 속성값이 될 FileAttribute를 나열
    - ex) “C:\Temp\file.txt” 파일에 대한 AsynchronousFileChannel을 다음과 같이 생성
    
    ```java
    ExecutorService executorService 
    		= Executors.newFixedThreadPool(
    				Runtime.getRuntime().availableProcessors();
    			);
    
    AsynchronousFileChannel fileChannel
    		= AsynchronousFileChannel.open(
    				Paths.get("C:/Temp/file.txt"),
    				EnumSet.of(StandardOpenOption.CREATE,
    									StandardOpenOption.WRITE),
    				executorService
    			);
    ```
    
    - `RunTime.getRuntime().availableProcessors()`는 CPU의 코어 수를 리턴
        - 쿼드 코어 CPU일 경우 4
        - 하이퍼 스레딩일 경우 8
    - `EnumSet.of()` 메소드는 매개값으로 나열된 열거 상수를 Set 객체에 담아 리턴
- AsynchronousFileChannel을 더 이상 사용하지 않을 경우에는 `close()` 메소드를 호출해서 닫아준다.
    
    ```java
    fileChannel.close();
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판