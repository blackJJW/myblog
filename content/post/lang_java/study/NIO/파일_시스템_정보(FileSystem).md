---
title: "[Java] NIO, 파일 시스템 정보(FileSystem)"
description: ""
date: "2023-01-23T15:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 운영체제의 파일 시스템은 FileSystem 인터페이스르 통해 접근 가능
- FileSystem 구현 객체는 FileSystems의 정적 메소드인 `getDefault()`로 얻을 수 있다.
    
    ```java
    FileSystem fileSystem = FileSystem.getDefault();
    ```
    
    ### FileSystem에서 제공하는 메소드
    
    | 리턴 타입 | 메소드(매개 변수) | 설명 |
    | --- | --- | --- |
    | Iterable<FileStore> | getFileStores() | 드라이버 정보를 가진 FileStorea 객체들을 리턴 |
    | Iterable<Path> | getRootDirectories() | 루트 디렉토리 정보를 가진 Path 객체들을 리턴 |
    | String | getSeperator() | 디렉토리 구분자 리턴 |
    
    ### FileStore는 드라이버를 표현한 객체로 다음과 같은 메소드를 제공
    
    | 리턴 타입 | 메소드(매개 변수) | 설명 |
    | --- | --- | --- |
    | long | getTotalSpace() | 드라이버 전체 공간 크기(단위 : 바이트) 리턴 |
    | long | getUnallocateSpace() | 할당되지 않은 공간 크기(단위 : 바이트) 리턴 |
    | long | getUsableSpace() | 사용 가능한 공간 크기, getUnallocateSpace()와 동일한 값 |
    | boolean | isReadOnly() | 읽기 전용 여부 |
    | String | name() | 드라이버명 리턴 |
    | String | type() | 파일 시스템 종류 |
- ex) 파일 시스템 정보 보기
    - FileSystemEx.java
    
    ```java
    import java.nio.file.FileStore;
    import java.nio.file.FileSystem;
    import java.nio.file.FileSystems;
    import java.nio.file.Path;
    
    public class FileSystemEx {
    
    	public static void main(String[] args) throws Exception {
    		FileSystem fileSystem = FileSystems.getDefault();
    		for(FileStore store : fileSystem.getFileStores()) {
    			System.out.println("드라이버명 : " + store.name());
    			System.out.println("파일시스템 : " + store.type());
    			System.out.println("전체 공간 : " + store.getTotalSpace());
    			System.out.println("사용 중인 공간 : " 
    						+ (store.getTotalSpace() - store.getUnallocatedSpace()) 
    						+ " 바이트");
    			System.out.println("사용 가능한 공간 : " 
    						+ store.getUsableSpace() + " 바이트");
    			System.out.println();
    		}
    		
    		System.out.println("파일 구분자 : " + fileSystem.getSeparator());
    		System.out.println();
    		
    		for(Path path : fileSystem.getRootDirectories()) {
    			System.out.println(path.toString());
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/파일_시스템_정보(FileSystem)/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판