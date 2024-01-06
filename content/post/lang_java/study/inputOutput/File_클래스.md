---
title: "[Java] 파일 입출력, File 클래스"
description: ""
date: "2023-01-07T14:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- IO 패키지(`java.io`)에서 제공하는 File 클래스는 파일 크기, 파일 속성, 파일 이름 등의 정보를 얻어내는 기능과 파일 생성 및 삭제 기능을 제공
- 디렉토리를 생성하고 디렉토리에 존재하는 파일 리스트를 얻어내는 기능도 있다.
- 파일의 데이터를 읽고 쓰는 기능은 지원하지 않는다.
- 파일의 입출력은 스트림을 사용해야 한다.
- ex) `C:\Temp` 디렉토리의 file.txt 파일을 File 객체로 생성하는 코드
    
    ```java
    File file = new File("C:\\Temp\\file.txt");
    File file = new File("C:/Temp/file.txt");
    ```
    
    - 디렉토리의 구분자는 운영체제마다 조금씩 다르다.
        - 윈도우에서는 `\` 또는 `/`를 사용 가능
        - 유닉스나 리눅스에서는 `/`를 사용
    - `File.seperator` 상수를 출력해보면 해당 운영체제에서 사용하는 디렉토리 구분자를 확인 가능
    - 만약 `\`를 디렉토리 구분자로 사용한다면 이스케이프 문자(`\\`)로 기술해야 한다.
- File 객체를 생성했다고 해서 파일이나 디렉토리가 생성되는 것은 아니다.
    - 생성자 매개값으로 주어진 경로가 유효하지 않더라도 컴파일 에러나 예외가 발생하지 않는다.
    - File 객체를 통해 해당 경로에 실제로 파일이나 디렉토리가 있는지 확인하려면 `exists()` 메소드를 호출 가능
    - 디렉토리 또는 파일이 파일 시스템에 존재한다면 true를 리턴하고 존재하지 않는다면 false를 리턴
    
    ```java
    boolean isExist = file.exists();
    ```
    
    - `exists()` 메소드의 리턴값이 false라면 `createNewFile()`, `mkdir()`, `mkdirs()` 메소드 파일 또는 디렉토리를 생성할 수 있다.
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | boolean | createNewFile() | 새로운 파일 생성 |
    | boolean | mkdir() | 새로운 디렉토리를 생성 |
    | boolean | mkdirs() | 경로상에 없는 모든 디렉토리를 생성 |
    | boolean | delete() | 파일 또는 디렉토리 삭제 |
    - 파일 또는 디렉토리가 존재할 경우에는 다음 메소드를 통해 정보를 얻어낼 수 있다.
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | boolean | canExecute() | 실행할 수 있는 파일인지 여부 |
    | boolean | canRead() | 읽을 수 있는 파일인지 여부 |
    | boolean | canWrite() | 수정 및 저장할 수 있는 파일인지 여부 |
    | String | getName() | 파일의 이름을 리턴 |
    | String | getParent() | 부모 디렉토리를 리턴 |
    | File | getParentFile() | 부모 디렉토리를 File 객체로 생성 후 리턴 |
    | String | getPath() | 전체 경로를 리턴 |
    | boolean | isDirectory() | 디렉토리인지 여부 |
    | boolean | isFile() | 파일인지 여부 |
    | boolean | isHidden() | 숨긴 파일인지 여부 |
    | long | lastModified() | 마지막 수정 날짜 및 시간을 리턴 |
    | long | length() | 파일의 크기를 리턴 |
    | String[] | list() | 디렉토리에 포함된 파일 및 서브디렉토리 목록 전부를 String 배열로 리턴 |
    | String[] | list(FilenameFilter filter) | 디렉토리에 포함된 파일 및 서브디렉토리 목록 전부를 String 배열로 리턴 |
    | File[] | listFiles() | 디렉토리에 포함된 파일 및 서브디렉토리 목록 중에 FilenameFilter에 맞는 것만 String 배열로 리턴 |
    | File[] | listFiles(FilenameFilter filter) | 디렉토리에 포함된 파일 및 서브디렉토리 목록 중에 FilenameFilter에 맞는 것만 File 배열로 리턴 |
- ex) C:\Temp 디렉토리에 Dir 디렉토리와 file1.txt, file2.txt, file3.txt 파일을 생성
    - Temp 디렉토리에 있는 파일 목록을 출력
    - FileEx.java
    
    ```java
    import java.io.File;
    import java.net.URI;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    
    public class FileEx {
    
    	public static void main(String[] args) throws Exception {
    		File dir = new File("C:/Temp/Dir");
    		File file1 = new File("C:/Temp/file1.txt");
    		File file2 = new File("C:/Temp/file2.txt");
    		File file3 = new File(new URI("file:///C:/Temp/file3.txt"));
    		                      // 파일 경로를 URI 객체로 생성해서 매개값으로 제공해도 된다.
    		
    		if(dir.exists() == false) { dir.mkdir(); }
    		if(file1.exists() == false) { file1.createNewFile(); }
    		if(file2.exists() == false) { file2.createNewFile(); }
    		if(file3.exists() == false) { file3.createNewFile(); }
    		
    		File temp = new File("C:/Temp");
    		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd  a  HH:mm");
    		File[] contents = temp.listFiles();
    		
    		System.out.println("날짜            시간       형태             크기    이름");
    		System.out.println("-------------------------------------------------------------------");
    		for(File file : contents) {
    			System.out.print(sdf.format(new Date(file.lastModified())));
    			if(file.isDirectory()) {
    				System.out.print("\t<DIR>\t\t\t" + file.getName());
    			} else {
    				System.out.print("\t\t\t" + file.length() + "\t" + file.getName());
    			}
    			System.out.println();
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/File_클래스/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판