---
title: "[Java] 파일 입출력, FileReader"
description: ""
date: "2023-01-07T17:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 텍스트 파일을 프로그램으로 읽어들일 때 사용하는 문자 기반 스트림
- 문자 단위로 읽기 때문에 텍스트가 아닌 그림, 오디오, 비디오 등의 파일은 읽을 수 없다.
- FileReader를 생성하는 두 가지 방법
    1. 전체 파일의 경로를 가지고 FileReader를 생성
    2. 읽어야할 파일이 객체로 이미 생성되어 있다면 두 번째 방법으로 좀 더 쉽게 FileReader를 생성 가능
    
    ```java
    // 방법 1
    FileReader fr = new FileReader("C:/Temp/file.txt");
    
    // 방법 2
    File file = new File("C:/Temp/file.txt");
    FileReader fr = new FileReader(file);
    ```
    
- FileReader 객체가 생성될 때 파일과 직접 연결이 된다.
    - 만약 파일이 존재하지 않으면 FileNotFoundException을 발생시키므로 try-catch문으로 예외 처리
- FileReader는 Reader의 하위 클래스이기 때문에 사용 방법이 Reader와 동일
    - 한 문자를 읽기 위해서 `read()` 메소드를 사용
    - 읽은 문자를 char 배열에 저장하기 위해서 `read(char[] cbuf)` 또는 `read(char[] cbuf, int off, int len)` 메소드를 사용
    - 전체 파일의 내용을 읽기 위해서는 이 메소드들을 반복 실행해서 -1이 나올 때까지 읽으면 된다.
    
    ```java
    FileReader fr = new FileReader("C:/Temp/file.txt");
    int readCharNo;
    char[] cbuf = new char[100];
    while((readCharNo = fr.read(cbuf)) != -1) {
       // 읽은 문자 배열(cbuf) 처리
    }
    fr.close();
    ```
    
    - 파일의 내용을 모두 읽은 후에는 `close()` 메소드를 호출해서 파일을 닫아준다.
- ex) FileReaderEx.java 소스파일을 읽고 콘솔에 출력
    - FileReaderEx.java
    
    ```java
    import java.io.FileReader;
    
    public class FileReaderEx {
    
    	public static void main(String[] args) throws Exception {
    		FileReader fr = new FileReader(
    				"D:{ path }\\FileReaderEx.java"
    				);
    		
    		int readCharNo;
    		char[] cbuf = new char[100];
    		while((readCharNo = fr.read(cbuf)) != -1) {
    			String data = new String(cbuf, 0, readCharNo);
    			System.out.print(data);
    		}
    		fr.close();
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/FileReader/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판