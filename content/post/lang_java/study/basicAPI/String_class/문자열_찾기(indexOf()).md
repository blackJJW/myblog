---
title: "[Java] String 클래스, String 메소드, 문자열 찾기(indexOf())"
description: ""
date: "2022-09-25T21:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `indexOf()` 메소드는 매개값으로 주어진 문자열이 시작되는 인덱스를 리턴
    - 만약 주어진 문자열이 포함되어 있지 않으면 -1을 리턴
    
    ```java
    String subject = "자바 프로그래밍";
    int index = subject.indexOf("프로그래밍");
    ```
    
    - index 변수에는 3이 저장, “자바 프로그래밍”에서 “프로그래밍” 문자열의 인덱스 위치가 3이기 때문
        
        ![Untitled](/images/lang_java/basicAPI/문자열_찾기(indexOf())/Untitled.png)
        
- indexOf() 메소드는 if문의 조건식에서 특정 문자열이 포함되어 있는지 여부에 따라 실행 코드를 달리할 때 자주 사용된다.
    
    ```java
    if( 문자열.indexOf("찾는문자열") != -1 ) {
    	// 포함되어 있는 경우
    } else {
    	// 포함되어 있지 않은 경우
    }
    ```
    
- ex) 문자열 포함 여부 조사
    - StringIndexOfEx.java
    
    ```java
    public class StringIndexOfEx {
    
    	public static void main(String[] args) {
    		String subject = "자바 프로그래밍";
    		
    		int location = subject.indexOf("프로그래밍");
    		System.out.println(location);
    		
    		if(subject.indexOf("자바") != -1) {
    			System.out.println("자바 관련");
    		} else {
    			System.out.println("자바 비관련");
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_찾기(indexOf())/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판