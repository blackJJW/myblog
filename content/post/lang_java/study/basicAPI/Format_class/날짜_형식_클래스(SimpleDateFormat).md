---
title: "[Java] 날짜 형식 클래스(SimpleDateFormat)"
description: ""
date: "2022-10-01T23:35:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- Date 클래스의 `toString()` 메소드는 영문으로 된 날짜를 리턴하는데 만약 특정 문자열 포맷으로 얻고 싶다면 `java.text.SimpleDateFormat` 클래스를 이용하면 된다.
- SimpleDateFormat 클래스도 날짜를 원하는 형식으로 표현하기 위해 패턴을 사용
    
    ### 패턴 작성에 사용되는 기호
    
    | 패턴 문자 | 의미 | 패턴 문자 | 의미 |
    | --- | --- | --- | --- |
    | y | 년 | H | 시(0 ~ 23) |
    | M | 월 | h | 시(1 ~ 12) |
    | d | 일 | K | 시(0 ~ 11) |
    | D | 월 구분이 없는 일(1 ~ 365) | k | 시(1 ~ 24) |
    | E | 요일 | m | 분 |
    | a | 오전/오후 | s | 초 |
    | w | 년의 몇 번째 주 | S | 밀리세컨드(1/1000초) |
    | W | 월의 몇 번째 주 |  |  |
    - 패턴에는 자릿수에 맞게 기호를 반복해서 작성 가능
        - ex) yyyy는 년도를 4자리로 표시
        - ex) MM, dd는 각각 달과 일을 2자리로 표시
    - 적용할 패턴을 작성했다면 이 패턴을 SimpleDateFormat의 생성자 매개값으로 지정해서 객체를 생성
        - 그리고 나서 format() 메소드를 호출해서 패턴이 적용된 문자열을 얻는다.
    
    ```java
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일");
    String strDate = sdf.format(new Date());
    ```
    
- ex) 날짜를 원하는 형식으로 출력
    - 다양한 패턴을 적용해서 얻은 문자열을 출력
    - SimpleDateFormatEx.java
    
    ```java
    import java.text.SimpleDateFormat;
    import java.util.Date;
    
    public class SimpleDateFormatEx {
    
    	public static void main(String[] args) {
    		Date now = new Date();
    		
    		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
    		System.out.println(sdf.format(now));
    		
    		sdf = new SimpleDateFormat("yyyy년 MM월 dd일");
    		System.out.println(sdf.format(now));
    		
    		sdf = new SimpleDateFormat("yyyy.MM.dd a HH:mm:ss");
    		System.out.println(sdf.format(now));
    		
    		sdf = new SimpleDateFormat("오늘은 E요일");
    		System.out.println(sdf.format(now));
    		
    		sdf = new SimpleDateFormat("올해의 D번째 날");
    		System.out.println(sdf.format(now));
    		
    		sdf = new SimpleDateFormat("이달의 d번째 날");
    		System.out.println(sdf.format(now));
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/날짜_형식_클래스(SimpleDateFormat)/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판