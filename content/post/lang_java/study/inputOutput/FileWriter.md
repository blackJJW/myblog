---
title: "[Java] 파일 입출력, FileWriter"
description: ""
date: "2023-01-08T17:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 텍스트 데이터를 파일에 저장할 때 사용하는 문자 기반 스트림
- 문자 단위로 저장하기 때문에 텍스트가 아닌 그림, 오디오, 비디오 등의 데이터를 파일로 저장할 수 없다.
- ex) FileWriter를 생성하는 두 가지 방법
    1. 전체 파일의 경로를 가지고 FileWriter을 생성
    2. 저장할 파일이 File 객체로 이미 생성되어 있다면 두 번째 방법으로 좀 더 쉽게 FileWriter를 생성 가능
    
    ```java
    // 방법 1
    FileWriter fw = new FileWriter("C:/Temp/file.txt");
    
    // 방법 2
    File file = new File("C:/Temp/file.txt");
    FileWriter fw = new FileWriter(file);
    ```
    
    - 위와 같이 FileWriter를 생성하면 지정된 파일이 이미 존재할 경우 그 파일을 덮어쓰게 된다.
        - 기존의 파일 내용은 사라지게 된다.
    - 기존의 파일 내용 끝에 데이터를 추가할 경우에는 FileWriter 생성자에게 두 번째 매개값으로 true를 주면 된다.
    
    ```java
    FileWriter fw = new FileWriter("C:/Temp/file.txt", true);
    FileWriter fw = new FileWriter(file, true);
    ```
    
- FileWriter는 Writer의 하위 클래스이기 때문에 사용 방법이 Writer와 동일
    - 한 문자를 저장하기 위해서 `write()` 메소드를 사용하고 문자열을 저장하기 위해서 `writer(String str)` 메소드를 사용
    
    ```java
    FileWriter fw = new FileWriter("C:/Temp/file.txt");
    String data = "저장할 문자열";
    fw.write(data);
    fw.flush();
    fw.close();
    ```
    
    - `write()` 메소드를 호출한 이후
        - `flush()` 메소드로 출력 버퍼에 있는 데이터를 파일로 완전히 출력
        - `close()` 메소드를 호출해서 파일을 닫아준다.
- ex) 문자열 데이터를 `C:\Temp\file.txt` 파일에 저장
    - FileWriterEx.java
    
    ```java
    import java.io.File;
    import java.io.FileWriter;
    
    public class FileWriterEx {
    
    	public static void main(String[] args) throws Exception {
    		File file = new File("C:/Temp/file.txt");
    		FileWriter fw = new FileWriter(file, true);
    		fw.write("FileWriter는 한글로 된" + "\r\n");
    		fw.write("문자열을 바로 출력할 수 있다." + "\r\n");
    		fw.flush();
    		fw.close();
    		System.out.println("파일에 저장됨.");
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/FileWriter/Untitled.png)
    
    ![Untitled](/images/lang_java/inputOutput/FileWriter/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판