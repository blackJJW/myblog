---
title: "[Java] 콘솔 입출력, Console 클래스"
description: ""
date: "2023-01-06T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 콘솔에서 입력받은 문자열을 쉽게 읽을 수 있도록 `java.io.Console` 클래스를 제공
- Console 객체를 얻기 위해서는 System의 정적 메소드인 `console()`을 호출
    
    ```java
    Console console = System.console();
    ```
    
- 주의할 점은 이클립스에서 실행하면 `System.console()` 메소드는 null을 리턴하기 때문에 반드시 명령 프롬프트에서 실행해야 한다.
    
    ![Untitled](/images/lang_java/inputOutput/Console_클래스/Untitled.png)
    
    ### Console 클래스에서 읽기 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | String | readLine( ) | Enter키를 입력하기 전의 모든 문자열을 읽음 |
    | char[ ] | readPassword( ) | 키보드 입력 문자를 콘솔에 보여주지 않고 문자열을 읽음 |
- ex) 콘솔로부터 아이디와 패스워드를 입력받아 출력
    - ConsoleEx.java
    
    ```java
    import java.io.Console;
    
    public class ConsoleEx {
    
    	public static void main(String[] args) {
    		Console console = System.console();
    		
    		System.out.print("아이디 : ");
    		String id = console.readLine();
    		
    		System.out.print("패스워드 : ");
    		char[] charPass = console.readPassword();
    		
    		// char[] 배열을 문자열로 생성
    		String strPassword = new String(charPass);
    		
    		System.out.println("---------------------");
    		
    		// id와 Password를 콘솔에 출력
    		System.out.println(id);
    		System.out.println(strPassword);
    	}
    
    }
    ```
    
    - ConsoleEx 클래스를 실행하려면 명령 프롬프트(cmd) 또는 PowersShell를 열고 해당 패키지가 시작하는 bin 디렉토리로 이동
    
    ```bash
    cd D:\ ~~~ \java_practice\inputOutput\bin
    ```
    
    - java 명령어로 실행
    
    ```bash
    java inputOutput.console.ConsoleEx
    ```
    
    - 아이디를 키보드로 입력할 때에는 입력 문자가 에코(echo) 출력
    - 패스워드는 입력 문자가 에코 출력되지 않아 보안상 유리
    
    ![Untitled](/images/lang_java/inputOutput/Console_클래스/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판