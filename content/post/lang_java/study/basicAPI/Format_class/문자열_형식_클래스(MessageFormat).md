---
title: "[Java] 문자열 형식 클래스(MessageFormat)"
description: ""
date: "2022-10-01T23:50:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- MessageFormat 클래스를 사용하면 문자열에 데이터가 들어갈 자리를 표시
    - 프로그램이 실행하면서 동적으로 데이터를 삽입해 문자열을 완성 가능
- ex) 회원 정보를 출력한다고 가정
    
    ```java
    회원 ID : blue
    회원 이름 : ABC
    회원 정보 : 010-123-4567
    ```
    
    - 만약 id, name, tel이라는 변수에 회원 정보가 저장되어 있을 경우
        - 다음과 같이 문자열 연결 연산자(+)로 출력할 문자열 생성 가능
        
        ```java
        String result = "회원 ID :"+id+"\n회원 이름 : "+name+"\n회원 정보 : "+tel;
        ```
        
        - MessageFormat 클래스를 사용하면 전체 문자열을 쉽게 예측 가능
        
        ```java
        String message = "회원 ID : {0} \n회원 이름 : {1} \n회원 전화 : {2}";
        String result = MessageFormat.format(message, id, name, tel);
        ```
        
- MessageFormat은 정적 `format()` 메소드를 호출해서 완성된 문자열을 리턴
    - format() 메소드의 첫 번째 매개값은 매개 변수화된 문자열을 지정
    - 두 번째 이후의 매개값은 인덱스 순서에 맞게 값을 나열
        - 값을 나열하는 대신, 배열을 대입해도 좋다.
        
        ```java
        String text = "회원 ID : {0} \n회원 이름 : {1} \n회원 전화 : {2}";
        Object[] arguments = { id, name, tel };
        String result = MessageFormat.format(text, arguments);
        ```
        
- ex) 매개 변수화된 문자열 형식
    - 매개 변수화된 문자열을 이용해서 회원 정보 및 SQL문을 콘솔에 출력
    - MessageFormatEx.java
    
    ```java
    import java.text.MessageFormat;
    
    public class MessageFormatEx {
    
    	public static void main(String[] args) {
    		String id =  "java";
    		String name = "ABC";
    		String tel = "010-123-4567";
    		
    		String text = "회원 ID : {0} \n회원 이름 : {1} \n회원 전화 : {2}";
    		String result1 = MessageFormat.format(text, id, name, tel);
    		System.out.println(result1);
    		System.out.println();
    		
    		String sql = "insert into member values( {0}, {1}, {2} )";
    		Object[] arguments = { "'java'", "'ABC'", "'010-123-4567'" };
    		String result2 = MessageFormat.format(sql,  arguments);
    		System.out.println(result2);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/문자열_형식_클래스(MessageFormat)/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판