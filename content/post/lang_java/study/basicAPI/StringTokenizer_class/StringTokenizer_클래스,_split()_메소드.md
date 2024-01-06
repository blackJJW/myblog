---
title: "[Java] StringTokenizer 클래스, split() 메소드"
description: ""
date: "2022-09-26T22:20:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 문자열이 특정 구분자(delimiter)로 연결되어 있을 경우
    - 구분자를 기준으로 부분 문자열을 분리하기 위해서는 String의 `split()` 메소드를 이용하거나, `java.util` 패키지의 StringTokenizer 클래스를 이용할 수 있다.
    - `split()`은 정규 표현식으로 구분
    - StringTokenizer는 문자로 구분

---

## `split()` 메소드

---

- String 클래스의 `split()` 메소드는 다음과 같이 호출되는데, 정규 표현식을 구분자로 해서 문자열을 분리한 후, 배열에 저장하고 리턴
    
    ```java
    String[] result = "문자열".split("정규표현식");
    ```
    
- ex) &, 쉼표( , ), - 를 제외하고 문자열인 “ABC”, “DEF”, “GHI”, “JKL”, “MNO”만 따로 뽑아내고 싶을 경우
    
    ```java
    ABC&DEF,GHI,JKL-MNO
    ```
    
    - &, 쉼표( , ), - 를 파이프( | ) 기호로 연결한 정규 표현식을 매개값으로 제공하면 `split()` 메소드는 이 기호들을 구분자로 해서 부분 문자열을 추출
        
        ```java
        String[] names = text.split("&|,|-")
        ```
        
    - StringSplitEx.java
    
    ```java
    public class StringSplitEx {
    
    	public static void main(String[] args) {
    		String text = "ABC&DEF,GHI,JKL-MNO";
    		
    		String[] txt = text.split("&|,|-");
    		
    		for (String tmp : txt) {
    			System.out.println(tmp);
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/StringTokenizer_class/StringTokenizer_클래스,_split()_메소드/Untitled.png)
    

---

## StringTokenizer 클래스

---

- 문자열이 한 종류의 구분자로 연결되어 있을 경우
    - StringTokenizer 클래스를 사용하면 손쉽게 문자열(토큰 : token)을 분리 가능
- StringTokenizer 객체를 생성할 때
    - 첫 번째 매개값으로 전체 문자열
    - 두 번째 매개값으로 구분자
    
    ```java
    StringTokenizer st = new StringTokenizer("문자열", "구분자");
    ```
    
- ex) 구분자를 생략할 경우 공백(Space)이 기본 구분자가 된다.
    - 문자열이 “/”로 구분되어 있을 경우, 다음과 같이 StringTokenizer 객체를 생성 가능
    
    ```java
    String text = "ABC/DEF/GHI";
    StringTokenizer st = new StringTokenizer(text, "/");
    ```
    
- StringTokenizer 객체가 생성되면 부분 문자열을 분리 가능
    - 다음 메소드들을 이용해서 전체 토큰 수, 남아 있는 토큰 여부를 확인한 다음, 토큰을 읽으면 된다.
    
    ![Untitled](/images/lang_java/basicAPI/StringTokenizer_class/StringTokenizer_클래스,_split()_메소드/Untitled%201.png)
    
    - `nextToken()` 메소드로 토큰을 하나 꺼내오면 StringTokenizer 객체에는 해당 토큰이 없어진다.
        - StringTokenizer 객체에서 더 이상 가져올 토큰이 없다면 `nextToken()` 메소드는 `java.util.NoSuchElementException` 예외를 발생시킴
        - `nextToken()` 메소드를 사용하기 전에 `hasMoreTokens()` 메소드로 꺼내올 토큰이 있는 지 조사한 후 `nextToken()` 메소드를 호출하는 것이 좋은 방법
- ex) StringTokenizer로 토큰 분리하기
    - 두 가지 방법으로 토큰을 추출하는 방법
    - StringTokenizerEx.java
    
    ```java
    import java.util.StringTokenizer;
    
    public class StringTokenizerEx {
    
    	public static void main(String[] args) {
    		String text = "ABC/DEF/GHI";
    		
    		// how1 : 전체 토큰 수를 얻어 for문으로 루핑
    		StringTokenizer st = new StringTokenizer(text, "/");
    		int countTokens = st.countTokens();
    		for(int i = 0; i < countTokens; i++) {
    			String token = st.nextToken();
    			System.out.println(token);
    		}
    		
    		System.out.println();
    		
    		// how2 : 남아 있는 토큰을 확인하고 while문으로 루핑
    		st = new StringTokenizer(text, "/");
    		while(st.hasMoreTokens()) {
    			String token = st.nextToken();
    			System.out.println(token);
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/StringTokenizer_class/StringTokenizer_클래스,_split()_메소드/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판