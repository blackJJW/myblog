---
title: "[Java] NIO, 경로 정의(Path)"
description: ""
date: "2023-01-23T14:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- NIO에서 제일 먼저 살펴봐야 할 API는 `java.nio.file.Path` 인터페이스
- Path는 IO의 `java.io.File` 클레스에 대응되는 NIO 인터페이스
    - NIO의 API에서 파일의 경로를 지정하기 위해 Path를 사용
- Path 구현 객체를 얻기 위해서는 `java.nio.file.Paths` 클래스의 정적 메소드인 `get()` 메소드를 호출
    
    ```java
    Path path = Paths.get(String first, String... more);
    Path path = Paths.get(URI uri);
    ```
    
    - `get()` 메소드의 매개값은 파일의 경로
        - 문자열로 지정할 수 있고 URI 객체로 지정할 수도 있다.
        - 문자열로 지정할 경우
            - 전체 경로를 한꺼번에 지정
            - 상위 디렉토리와 하위 디렉터리를 나열해서 지정
    
    ex) “C:\Temp\dir\file.txt” 경로를 이용해서 Path 객체를 얻는 방법
    
    ```java
    Path path = Paths.get("C:/Temp/dir/file.txt");
    Path path = Paths.get("C:/Temp/dir", "file.txt");
    Path path = Paths.get("C:", "Temp", "dir", "file.txt");
    ```
    
    - 파일의 경로는 절대 경로와 상대 경로를 모두 사용할 수 있다.
        - 만약 현재 디렉토리 위치가 “C:\Temp”일 경우
            - “C:\Temp\dir\file.txt”는 다음과 같이 지정 가능
    
    ```java
    Path path = Paths.get("dir/file.txt");
    Paht path = Paths.get("./dir/file.txt");
    ```
    
    - 현재 위치가 “C:\Temp\dir1”이라면 “C:\Temp\dir2\file.txt”는 다음과 같이 지정 가능
    
     
    
    ```java
    Path path = Paths.get("../dir2/file.txt");
    ```
    
- Path 인터페이스에는 파일 경로에서 얻을 수 있는 여러 가지 정보를 제공해주는 메소드가 있다.

![Untitled](/images/lang_java/NIO/경로_정의(Path)/Untitled.png)

- ex) 상대 경로를 이용해서 소스 파일에 대한 Path 객체를 얻고, 파일명, 부모 디렉토리명, 중첩 경로 수, 경로상에 있는 모든 디렉토리를 출력
    - PathEx.java
    
    ```java
    import java.nio.file.Path;
    import java.nio.file.Paths;
    import java.util.Iterator;
    
    public class PathEx {
    
    	public static void main(String[] args) throws Exception {
    		Path path = Paths.get("src/nio/PathEx.java");
    		System.out.println("[ 파일명 ] " + path.getFileName());
    		System.out.println("[ 부목 디렉토리명 ] : " + path.getParent().getFileName());
    		System.out.println("중첩 경로수 : " + path.getNameCount());
    		
    		System.out.println();
    		for(int i = 0; i < path.getNameCount(); i++) {
    			System.out.println(path.getName(i));
    		}
    		
    		System.out.println();
    		Iterator<Path> iterator = path.iterator();
    		while(iterator.hasNext()) {
    			Path temp = iterator.next();
    			System.out.println(temp.getFileName());
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/NIO/경로_정의(Path)/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판