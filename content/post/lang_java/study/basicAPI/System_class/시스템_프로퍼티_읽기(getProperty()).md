---
title: "[Java] System 클래스, 시스템 프로퍼티 읽기(getProperty())"
description: ""
date: "2022-09-22T21:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 시스템 프로퍼티(System Property)는 JVM이 시작할 때 자동 설정되는 시스템의 속성값을 말한다.
    - ex) OS의 종류 및 자바 프로그램을 실행시킨 사용자 아이디, JVM 버전, OS에서 사용되는 파일 경로 구분자 등
- 시스템 프로퍼티는 키(key)와 값(value)으로 구성
    
    
    | 키(key) | 설명 | 값(value) |
    | --- | --- | --- |
    | java.version | 자바의 버전 | 1.8.0_20 |
    | java.home | 사용하는 JRE의 파일 경로 | <jdk 설치경로>\jre |
    | os.name | Operating system name | Windows 7 |
    | file.separator | File separator (”/” on UNIX) | \ |
    | user.name | 사용자의 이름 | 사용자계정 |
    | user.home | 사용자의 홈 디렉터리 | C:\Users\사용자계정 |
    | user.dir | 사용자가 현재 작업 중인 디렉터리 경로 | 다양 |
- 시스템 프로퍼티를 읽어오기 위해서는 `System.getProperty()` 메소드를 이용
    - 시스템의 프로퍼티의 키 이름을 매개값으로 받고, 해당 키에 대한 값을 문자열로 리턴
    
    ```java
    String value = System.getProperty(String key);
    ```
    
- ex) 시스템 프로퍼티 읽기
    - OS 이름, 사용자 이름, 사용자 홈 디렉터리를 알아내고 출력
    - 모든 시스템 프로퍼티를 키와 값으로 출력
    - GetPropertyEx.java
    
    ```java
    import java.util.Properties;
    import java.util.Set;
    
    public class GetPropertyEx {
    
    	public static void main(String[] args) {
    		String osName = System.getProperty("os.name");
    		String userName = System.getProperty("user.name");
    		String userHome = System.getProperty("user.home");
    		
    		// 개별 속성 읽기
    		System.out.println("OS 이름 : " + osName);
    		System.out.println("사용자 이름 : " + userName);
    		System.out.println("사용자 홈디렉터리 : " + userHome);
    		
    		System.out.println("---------------------------");
    		System.out.println(" [ key ] value");
    		System.out.println("---------------------------");
    		
    		// 모든 속성의 키와 값을 출력
    		Properties props = System.getProperties();
    		Set keys = props.keySet();
    		for(Object objKey : keys) {
    			String key = (String) objKey;
    			String value = System.getProperty(key);
    			System.out.println("[ " + key + " ]" + value);
    		}
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/시스템_프로퍼티_읽기(getProperty())/Untitled.png)
    
    - `System.getProperties()` 메소드는 모든(키, 값) 쌍을 저장하고 있는 Properties 객체를 리턴
        - 이 객체의 `keySet()` 메소드를 호출하면 키만으로 구성된 Set 객체를 얻을 수 있다.
    - for문은 Set객체로부터 키를 하나씩 얻어내어 문자열로 변환한 다음, `System.getProperty()` 메소드로 값을 얻어 키와 값을 모두 출력

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판