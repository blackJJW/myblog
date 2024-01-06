---
title: "[Java] 숫자 형식 클래스(DecimalFormat)"
description: ""
date: "2022-10-01T20:50:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- DecimalFormat은 숫자 데이터를 원하는 형식으로 표현하기 위해서 패턴을 사용
    
    ### 패턴의 예
    
    ![Untitled](/images/lang_java/basicAPI/숫자_형식_클래스(DecimalFormat)/Untitled.png)
    
    - 적용할 패턴을 선택했다면 DecimalFormat 생성자 매개갑스로 지정해서 객체를 생성하면 된다.
        - 그리고 나서 `format()` 메소드를 호출해서 패턴이 적용된 문자열을 얻으면 된다.
        
        ```java
        DecimalFormat df = new DecimalFormat("#,###.0");
        String result = df.format(1234567.89);
        ```
        
- ex) 숫자를 원하는 형익으로 출력
    - 다양한 패턴을 적용해서 얻은 문자열을 출력
    - DecimalFormatEx.java
    
    ```java
    import java.text.DecimalFormat;
    
    public class DecimalFormatEx {
    
    	public static void main(String[] args) {
    		double num = 1234567.89;
    		
    		DecimalFormat df = new DecimalFormat("0");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("0.0");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("0000000000.00000");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("#");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("#.#");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("##########.#####");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("#.0");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("+#.0");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("-#.0");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("#,###.0");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("0.0E0");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("+#,### ; -#,###");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("#.# %");
    		System.out.println(df.format(num));
    		
    		df = new DecimalFormat("\u00A4 #,###");
    		System.out.println(df.format(num));
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/숫자_형식_클래스(DecimalFormat)/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판