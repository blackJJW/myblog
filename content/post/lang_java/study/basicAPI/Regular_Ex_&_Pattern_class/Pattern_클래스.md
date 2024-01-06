---
title: "[Java] Pattern 클래스"
description: ""
date: "2022-09-27T23:20:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 정규 표현식으로 문자열을 검증하는 방법
    - 문자열을 정규 표현식으로 검증하는 기능은 `java.util.regex.Pattern` 클래스의 정적 메소드인 `matches()` 메소드가 제공
    
    ```java
    boolean result = Pattern.matches("정규식", "검증할 문자열");
    ```
    
    - 첫 번째 매개값은 정규 표현식
    - 두 번째 매개값은 검증할 문자열
    - 검증 후 결과가 boolean 타입으로 리턴
- ex) 문자열 검증
    - 전화번호와 이메일을 검증하는 코드
    - PatternEx.java
    
    ```java
    import java.util.regex.Pattern;
    
    public class PatternEx {
    
    	public static void main(String[] args) {
    		String regExp = "(02|010)-\\d{3,4}-\\d{4}";
    		String data = "010-123-4567";
    		boolean result = Pattern.matches(regExp, data);
    		
    		if(result) {
    			System.out.println("정규식과 일치");
    		} else {
    			System.out.println("정규식과 불일치");
    		}
    		
    		regExp = "\\w+@\\w+\\.\\w+(\\.\\w+)?";
    		data = "abc@abcde.com";
    		result = Pattern.matches(regExp, data);
    		
    		if(result) {
    			System.out.println("정규식과 일치");
    		} else {
    			System.out.println("정규식과 불일치");
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/Pattern_클래스/Untitled.png)
    
    ### 주의할 점
    
    - `\\d{3, 4}` 와 같이 쉼표 뒤에 공백이 있으면 예외 발생
    
    ![Untitled](/images/lang_java/basicAPI/Pattern_클래스/Untitled%201.png)
    
    - `\\d{3,4}` 와 같이 쉼표 뒤에 공백이 없어야 된다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판