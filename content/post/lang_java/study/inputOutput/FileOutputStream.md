---
title: "[Java] 파일 입출력, FileOutputStream"
description: ""
date: "2023-01-07T16:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 바이트 단위로 데이터를 파일에 저장할 때 사용하는 바이트 기반 출력 스트림
- 바이트 단위로 저장하기 때문에 그림, 오디오, 비디오, 텍트트 등 모든 종류의 데이터를 파일로 저장할 수 있다.
- FileOutputStream을 생성하는 두 가지 방법
    1. 파일의 경로를 가지고 FileOutputStream을 생성
    2. 저장할 파일이 File 객체로 이미 생성되어 있다면, 좀 더 쉽게 FileOutputStream을 생성할 수 있다.
    
    ```java
    // 방법1
    FileOutputStream fos = new FileOutputStream("C:/Temp/image.gif");
    
    // 방법2
    File file = new File("C:/Temp/image.gif");
    FileOutputStream fos = new FileOutputStream(file);
    ```
    
- 주의할 점은 파일이 이미 존재할 경우, 데이터를 출력하면 파일을 덮어쓰게 된다.
    - 기존의 내용은 사라지게 된다.
- 기존의 파일 내용 끝에 데이터를 추가할 경우에는 FileOutputStream 생성자의 두 번째 매개값을 true로 한다.
    
    ```java
    FileOutputStream fos = 
              new FileOutputStream("C:/Temp/image.gif", true);
    FileOutputStream fos = new FileOutputStream(file, true); 
    ```
    
- FileOutputStream은 OutputStream의 하위 클래스이기 때문에 사용 방법이 OutputStream과 동일
    - 한 바이트를 저장하기 위해
        - `write()` 메소드를 사용
    - 바이트 배열을 한꺼번에 저장하기 위해
        - `write(byte[] b)` 또는 `write(byte[] b, int off, int len)` 메소드를 사용
    
    ```java
    FileOutputStrem fos = new FileOutputStream("C:/Temp/image.gif");
    byte[] data = ...;
    fos.write(data);
    fos.flush();
    fos.close();
    ```
    
    - `write()` 메소드를 호출한 이후에 `flush()` 메소드로 출력 버퍼에 잔류하는 데이터를 완전히 출력
    - `close()` 메소드를 호출해서 파일을 닫아준다.
- ex) 원본 파일을 타겟 파일로 복사하는 예제
    - 복사 프로그램의 원리는 원본 파일에서 읽은 바이트를 바로 타겟 파일로 저장하는 것
        - FileInputStream에서 읽은 바이트를 바로 FileOutputStream으로 저장
    - FileOutputStreamEx.java
    
    ```java
    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    
    public class FileOutputStreamEx {
    
    	public static void main(String[] args) throws Exception {
    		String originalFileName = 
    				"D:\\Users\\jjinw\\Desktop\\GitHub\\study\\java_practice\\inputOutput\\src\\fileInputOutput\\p-2.png";
    		
    		String targetFileName = "C:/Temp/p-2.png";
    		
    		FileInputStream fis = new FileInputStream(originalFileName);
    		FileOutputStream fos = new FileOutputStream(targetFileName);
    		
    		int readByteNo; // 실제로 읽은 바이트 수가 저장될 변수
        // 실제로 읽은 바이트가 저장되는 배열
        byte[] readBytes = new byte[100];
    		while( (readByteNo = fis.read(readBytes)) != -1 ) {
    			fos.write(readBytes, 0, readByteNo);
          // 실제로 읽은 바이트 수만큼만 FileOutputStream의 
          // write(byte[] b, int off, int len) 메소드로 저장
          // b = readBytes, off = 0, len = readByteNo
    		}
    		
    		fos.flush();
    		fos.close();
    		fis.close();
    		
    		System.out.println("복사 완료");
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/FileOutputStream/Untitled.png)
    
    ![Untitled](/images/lang_java/inputOutput/FileOutputStream/Untitled%201.png)
    
    - FileInputStream의 `read(byte[] b)` 메소드로 한 번에 100바이트씩를 읽어 readBytes에 저장하고 100을 readByteNo에 저장
        - readByteNo가 -1이 아닌지 검사
        - 이렇게 루핑을 돌면서 마지막 루핑 시에는 100개보다 작은 바이트를 읽어 readBytes에 저장
            - 바이트 수를 readByteNo에 저장
    
    ex) 파일의 사이즈가 520 바이트라면 while문은 6번 루핑
    
    - 마지막 루핑 시에는 20 바이트만 읽어 readBytes에 저장
        - 20을 readByteNo에 저장

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판