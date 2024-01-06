---
title: "[Java]  Map 컬렉션, Properties"
description: ""
date: "2022-11-13T16:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Properties는 Hashtable의 하위 클래스이기 때문에 Hashtable의 모든 특징을 그대로 가지고 있다.
    - 차이점
        - Hashtable은 키와 값을 다양한 타입으로 지정 가능
        - Properties는 기와 값을 String으로 제한한 컬렉션
- Properites는 애플리케이션의 옵션 정보, 데이터베이스 연결 정보 그리고 국제화 정보가 저장된 프로퍼티(~.properties) 파일을 읽을 때 주로 사용
- 프로퍼티 파일은 키와 값이 `=` 기호로 연결되어 있는 텍스트 파일로 ISO 8859-1 문자셋으로 저장
    - 이 문자셋으로 직접 표현할 수 없는 한글은 유니코드(Unicode)로 변환되어 저장
    
    ![Untitled](/images/lang_java/collectionFramework/Map_컬렉션/Properties/Untitled.png)
    
- ex) `키=값`으로 구성된 프로퍼티
    - 데이터베이스 연결 정보가 있는 프로퍼티 파일의 내용을 보여줌
    - driver, url, username, password는 키가 되고 그 뒤의 문자열을 값이 된다.
    - database.properties
    
    ```java
    driver = oracel.jdbc.OracleDriver
    url = jdbc:oracle:thin:@localhost:1521:orcl
    username = scott
    password = tiger
    ```
    
- 프로퍼티 파일을 읽기 위해서는 Properties 객체를 생성
    - `load()` 메소드를 호출하면 된다.
- `load()` 메소드는 프로퍼티 파일로부터 데이터를 읽기 위해 FileReader 객체를 매개값으로 받는다.
    
    ```java
    Properties properties = new Properties();
    properties.load(new FileReader("C:/~/database.properties"));
                                 // 프로퍼티 파일 경로 
    ```
    
- 프로퍼티 타일은 일반적으로 클래스 파일(~.class)과 함께 저장
    - 클래스 파일을 기준으로 상대 경로를 이용해서 프로퍼티 파일의 경로를 얻으려면 Class의 `getResource()` 메소드를 이용하면 된다.
    - `getResource()`는 주어진 파일의 상대 경로를 URL 객체로 리턴
        - URL의 `getPath()`는 파일의 절대 경로를 리턴
- ex) 클래스 파일과 동일한 위치에 있는 “database.properties” 파일을 읽고 Properties 객체를 생성하는 코드
    
    ```java
    String path = 클래스.class.getResource("database.properties").getPath();
    path = URLDecoder.decode(path, "utf-8"); // 경로에 한글이 있을 경우 한글을 복원
    Properties properties = new Properties();
    properties.load(new FileReader(path));
    ```
    
    - 만약 다른 패키지에 프로퍼티 파일이 있을 경우 경로 구분자로 “`/`”를 사용
        - ex) `A.class`가 `com.mycompany` 패키지에 있음
            - `database.properties` 파일이 `com.mycompany.config` 패키지에 있을 경우
            - 프로퍼티 파일의 절대 경로는 다음과 같이 얻을 수 있다.
            
            ```java
            String path = A.class.getResource("config/database.properties").getPath();
            ```
            
- Properties 객체에서 해당 키의 값을 읽으려면 `getProperty()` 메소드를 사용해야 한다.
    - Properties도 Map 컬렉션이므로 `get()` 메소드로 값을 얻을 수 있다.
    - `get()` 메소드는 값을 Object 타입으로 리턴하므로 강제 타입 변환해서 String을 얻어야 하기 때문에 일반적으로 `getProperty()` 메소드를 사용
    
    ```java
    String value = properties.getProperty("key");
    ```
    
- ex) 프로퍼티 파일로부터 읽기
    - `database.properties` 파일로부터 값을 읽어 출력
    - PropertiesEx.java
    
    ```java
    import java.io.FileReader;
    import java.net.URLDecoder;
    import java.util.Properties;
    
    public class PropertiesEx {
    
    	public static void main(String[] args) throws Exception {
    		Properties properties = new Properties();
    		String path = PropertiesEx.class.getResource("database.properties").getPath();
    		path = URLDecoder.decode(path, "utf-8");
    		properties.load(new FileReader(path));
    		
    		String driver = properties.getProperty("driver");
    		String url = properties.getProperty("url");
    		String username = properties.getProperty("username");
    		String password = properties.getProperty("password");
    		
    		System.out.println("driver : " + driver);
    		System.out.println("url : " + url);
    		System.out.println("username : " + username);
    		System.out.println("password : " + password);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/Map_컬렉션/Properties/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판