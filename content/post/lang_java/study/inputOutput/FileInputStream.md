---
title: "[Java] 파일 입출력, FileInputStream"
description: ""
date: "2023-01-07T15:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 파일로부터 바이트 단위로 읽어들일 때 사용하는 바이트 기반 입력 스트림
- 바이트 단위로 읽기 때문에 그림, 오디오, 비디오, 텍스트 파일 등 모든 종류의 파일을 읽을 수 있다.
- ex) FileInputStream을 생성하는 두 가지 방법
    
    ```java
    // 첫 번째 방법
    FileInputStream fis = new FileInputStream("C:/Temp/image.gif");
    
    // 두 번째 방법
    File file = new File("C:/Temp/image.gif");
    FileInputStream fis = new FileInputStream(file);
    ```
    
    - 첫 번째 방법은 문자열로된 파일의 경로를 가지고 FileInputStream을 생성
    - 만약 읽어야 할 파일이 File 객체로 이미 생성되어 있다면 두 번째 방법으로 좀 더 쉽게 FileInputStream을 생성할 수 있다.
    - FileInputStream 객체가 생성될 때 파일과 직접 연결이 된다.
        - 파일이 존재하지 않으면 FileNotFoundException을 발생시키므로 try-catch문으로 예외 처리를 해야 한다.
- FileInputStream은 InputStream의 하위 클래스이기 때문에 사용 방법이 InputStream과 동일
    - 한 바이트를 읽기 위해
        - `read()` 메소드를 사용
    - 읽은 바이트를 byte 배열에 저장하기 위해
        - `read(byte[] b)` 또는 `read(byte[] b, int off, int len)` 메소드를 사용
    - 전체 파일의 내용을 읽기 위해서는 이 메소드들을 반복 실행해서 -1이 나올때까지 읽으면 된다.
    - 파일의 내용을 모두 읽은 후에는 `close()` 메소드를 호출해서 파일을 닫아준다.
    
    ```java
    FileInputStream fis = new FileInputStream("C:/Temp/image.gif");
    int readByteNo;
    byte[] readBytes = new byte[100];
    while ( (readByteNo = fis.read(readBytes) ) != -1 ) {
    	// 읽은 바이트 배열(readBytes)을 처리
    }
    fis.close();
    ```
    
- ex) FileInputStreamEx.java 소스 파일을 읽고 콘솔에 보여주는 예제
    - FileInputStreamEx.java
    
    ```java
    import java.io.FileInputStream;
    
    public class FileInputStreamEx {
    
    	public static void main(String[] args) {
    		try {
    			FileInputStream fis = new FileInputStream("D:\\ -- path -- \\FileInputStreamEx.java");
    			int data;
    			
    			// 1byte씩 읽고 콘솔에 출력
    			while ( (data = fis.read() ) != -1) {
    				System.out.write(data);
    			}
    			fis.close();
    		} catch(Exception e) {
    			e.printStackTrace();
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/FileInputStream/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판