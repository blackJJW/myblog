---
title: "[Java] System 클래스, 환경 변수 읽기(getenv())"
description: ""
date: "2022-09-22T22:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 대부분의 OS는 실행되는 프로그램들에게 유용한 정보를 제공할 목적으로 환경 변수(Environment Variable)를 제공
    - 환경 변수는 프로그램상의 변수가 아니라 OS에서 이름(Name)과 값(Value)으로 관리되는 문자열 정보
    - OS가 설치될 때 기본적인 내용이 설정
    - 사용자가 직접 설정하거나 응용 프로그램이 설치될 때 자동적으로 추가 설정되기도 한다.
    
    ### Windows 환경 변수
    
    ![Untitled](/images/lang_java/basicAPI/환경_변수_읽기(getenv())/Untitled.png)
    
- 자바 프로그램에서는 환경 변수의 갑시 필요한 경우 `System.getenv()` 메소드를 사용
    - 매개값으로 환경 변수 이름을 주면 값을 리턴
    
    ```java
    String value = System.getenv(String name);
    ```
    
- ex) JAVA_HOME 환경 변수 값 얻기
    - SystemEnvEx.java
    
    ```java
    public class SystemEnvEx {
    
    	public static void main(String[] args) {
    		String javaHome = System.getenv("JAVA_HOME");
    		System.out.println("JAVA_HOME : " + javaHome);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/환경_변수_읽기(getenv())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판