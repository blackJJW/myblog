---
title: "[Java] String 클래스, String 메소드, 문자열 대치(replace())"
description: ""
date: "2022-09-25T21:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `replace()` 메소드는 첫 번째 매개값인 문자열을 찾아 두 번째 매개값인 문자열로 대치한 새로운 문자열을 생성하고 리턴
    
    ```java
    String oldStr = "자바 프로그래밍";
    String newStr = oldStr.replace("자바", "JAVA");
    ```
    
    - String 객체의 문자열은 변경이 불가한 특성을 갖기 때문에 `replace()` 메소드가 리턴하는 문자열은 원래 문자열의 수정본이 아니라 완전히 새로운 문자열
    - newStr 변수는 새로 생성된 “JAVA 프로그래밍” 문자열을 참조
    
    ![Untitled](/images/lang_java/basicAPI/문자열_대치(replace())/Untitled.png)
    
- ex) 문자열 대치하기
    - StringReplaceEx.java
    
    ```java
    public class StringReplaceEx {
    
    	public static void main(String[] args) {
    		String oldStr = "자바는 객체지향언어. 자바는 풍부한 API를 지원";
    		String newStr = oldStr.replace("자바","JAVA");
    		System.out.println(oldStr);
    		System.out.println(newStr);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_대치(replace())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판