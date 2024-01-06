---
title: "[Java] Date 클래스"
description: ""
date: "2022-10-01T12:50:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Date는 날짜를 표현하는 클래스
- 객체 간에 날짜 정보를 주고 받을 때 주로 사용
- 여러 개의 생성자가 선언되어 있지만 대부분 Deprecated(비권장)되어 현재는 `Date()` 생성자만 주로 사용
    - `Date()` 생성자는 컴퓨터의 현재 날짜를 읽어 Date 객체로 만든다.
    
    ```java
    Date now = new Date();
    ```
    
- 현재 날짜를 문자열로 얻고 싶다면 `toString()` 메소드를 사용
    - `toString()` 메소드는 영문으로 된 날짜를 리턴
    - 특정 문자열 포맷으로 얻고 싶다면 `java.text.SimpleDateFormat` 클래스를 이용
- ex) 현재 날짜를 출력
    - DateEx.java
    
    ```java
    import java.text.SimpleDateFormat;
    import java.util.Date;
    
    public class DateEx {
    
    	public static void main(String[] args) {
    		Date now = new Date();
    		String strNow1 = now.toString();
    		System.out.println(strNow1);
    		
    		SimpleDateFormat sdf = 
    				new SimpleDateFormat("yyyy년 MM월 dd일 hh시 mm분 ss초");
    		String strNow2 = sdf.format(now);
    		System.out.println(strNow2);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/Date_클래스/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판