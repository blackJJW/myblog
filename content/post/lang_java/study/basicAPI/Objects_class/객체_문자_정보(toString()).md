---
title: "[Java] Objects 클래스, 객체 문자 정보(toString())"
description: ""
date: "2022-09-21T01:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `Objects.toString()`은 객체의 문자 정보를 리턴
    - 다음 두 가지로 오버로딩되어 있다.
    
    | 리턴 타입 | 메소드(매개 변수) | 설명 |
    | --- | --- | --- |
    | String | toString(Object o) | not null → o.toString(), null → “null” |
    | String | toString(Object o, String nullDefault) | not null → o.toString(), null → nullDefault |
    - 첫 번째 매개값이 not null이면 `toString()`으로 얻은 값을 리턴
        - null이면 “null” 또는 두 번째 매개값인 nullDefault를 리턴
- ex) 객체 문자 정보
    - ToStringEx.java
    
    ```java
    import java.util.Objects;
    
    public class ToStringEx {
    
    	public static void main(String[] args) {
    		String str1 = "ABC";
    		String str2 = null;
    		
    		System.out.println(Objects.toString(str1));
    		System.out.println(Objects.toString(str2));
    		System.out.println(Objects.toString(str2, "이름이 없음"));
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/객체_문자_정보(toString())_2/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판