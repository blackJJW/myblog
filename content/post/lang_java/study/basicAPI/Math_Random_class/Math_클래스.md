---
title: "[Java] Math 클래스"
description: ""
date: "2022-09-30T23:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `java.lang.Math` 클래스는 수학 계산에 사용할 수 있는 메소드를 제공
- Math 클래스가 제공하는 메소드는 모두 정적(static)이므로 Math 클래스로 바로 사용 가능

### Math 클래스가 제공하는 메소드

![Untitled](/images/lang_java/basicAPI/Math_클래스/Untitled.png)

- ex) Math의 수학 메소드
    - MathEx.java
    
    ```java
    public class MathEx {
    
    	public static void main(String[] args) {
    		int v1 = Math.abs(-5);
    		double v2 = Math.abs(-3.14);
    		System.out.println("v1 = " + v1);
    		System.out.println("v2 = " + v2);
    		
    		double v3 = Math.ceil(5.3);
    		double v4 = Math.ceil(-5.3);
    		System.out.println("v3 = " + v3);
    		System.out.println("v4 = " + v4);
    		
    		double v5 = Math.floor(5.3);
    		double v6 = Math.floor(-5.3);
    		System.out.println("v5 = " + v5);
    		System.out.println("v6 = " + v6);
    		
    		int v7 = Math.max(5, 9);
    		double v8 = Math.max(5.3, 2.5);
    		System.out.println("v7 = " + v7);
    		System.out.println("v8 = " + v8);
    		
    		int v9 = Math.min(5, 9);
    		double v10 = Math.min(5.3, 2.5);
    		System.out.println("v9 = " + v9);
    		System.out.println("v10 = " + v10);
    		
    		double v11 = Math.random();
    		System.out.println("v11 = " + v11);
    		
    		double v12 = Math.rint(5.3);
    		double v13 = Math.rint(5.7);
    		System.out.println("v12 = " + v12);
    		System.out.println("v13 = " + v13);
    		
    		long v14 = Math.round(5.3);
    		long v15 = Math.round(5.7);
    		System.out.println("v14 = " + v14);
    		System.out.println("v15 = " + v15);
    		
    		double value = 12.3456;
    		double temp1 = value * 100;
    		long temp2 = Math.round(temp1);
    		double v16 = temp2 / 100.0;
    		System.out.println("v16 = " + v16);
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/Math_클래스/Untitled%201.png)
    
    - `round()` 메소드는 항상 소수점 첫째자리에서 반올림해서 정수값을 리턴
        - 만약 원하는 소수 자릿수에서 반올림된 값을 얻기 위해서는 반올림할 자릿수가 소수점 첫째 자리가 되도록 $10^{n}$을 곱한 후, `round()` 메소드의 리턴값을 얻는다.
        - 그리고 나서 다시 $10^{n}.0$을 나눠주면 된다.
    - `Math.random()` 메소드는 0.0과 1.0 사이의 범위에 속하는 하나의 double 타입의 값을 리턴
        - 0.0은 범위에 포함, 1.0은 포함되지 않는다.
        
        ```java
        0.0 <= Math.random() < 1.0
        ```
        
        - `Math.random()`을 활용해서 1부터 10까지의 정수 난수를 얻고 싶을 경우
            1. 각 변에 10을 곱하면 0.0 ≤ … < 10.0 사이의 범위에 속하는 하나의 double 타입의 값을 얻을 수 있다.
                
                ```java
                0.0 * 10 <= Math.random() * 10 < 1.0 * 10
                ```
                
            2. 각 변을 int 타입으로 강제 타입 변환하면 0 ≤ … < 10 사이의 범위에 속하는 하나의 int 타입의 값을 얻을 수 있다.
                
                ```java
                (int) (0.0 * 10) <= (int) (Math.random() * 10) < (int) (1.0 * 10)
                ```
                
            3. 각 변에 1을 더하면 1 ≤ … < 11 사이의 범위에 속하는 하나의 정수를 얻게 된다.
                
                ```java
                (int) (0.0*10) + 1 <= (int) (Math.random()*10)  + 1 < (int) (1.0*10) + 1
                ```
                
            4. start ≤ … < (start + n) 범위에 속하는 하나의 정수를 얻기 위한 연산식을 생성
                
                ```java
                int num = (int) (Math.random() * n) + start;
                ```
                
            
            ### [예제1] 주사위 번호 뽑기
            
            ```java
            int num = (int) (Math.random() * 6) + 1;
            ```
            
            ### [예제2] 로또 번호 뽑기
            
            ```java
            int num = (int) (Math.random() * 45) + 1;
            ```
            
- ex) 임의의 주사위의 눈 얻기
    - MathRandomEx 클래스는 한 번 던져서 나오는 주사위의 눈을 `Math.random()` 메소드를 이용해서 얻음
    - MathRandomEx.java
    
    ```java
    public class MathRandomEx {
    
    	public static void main(String[] args) {
    		int num = (int) (Math.random() + 6) + 1;
    		System.out.println("주사위 눈 : " + num);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/Math_클래스/Untitled%202.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판