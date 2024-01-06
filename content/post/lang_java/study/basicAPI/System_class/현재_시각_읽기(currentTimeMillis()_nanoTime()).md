---
title: "[Java] System 클래스, 현재 시각 읽기(currentTimeMillis(), nanoTime())"
description: ""
date: "2022-09-22T21:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- System 클래스의 `currentTimeMillis()` 메소드와 `nanoTime()` 메소드
    - 컴퓨터의 시계로부터 현재 시간을 읽어서 밀리세컨드($1/10^3$초) 단위와 나노세컨드 ($1/10^9$초) 단위의 long 값을 리턴
    
    ```java
    long time = System.currentTimeMillis();
    long time = System.nanoTime();
    ```
    
    - 리턴값은 주로 프로그램의 실행 소요 시간 측정에 사용
    - 프로그램 시작 시 시각을 읽고, 프로그램이 끝날 때 시각을 읽어서 차이를 구하면 프로그램 실행 소요 시간이 나온다.
- ex) 프로그램 실행 소요 시간 구하기
    - for문을 사용해서 1부터 1000000까지의 합을 구하는 데 걸린 시간을 출력
    - SystemTimeEx.java
    
    ```java
    public class SystemTimeEx {
    
    	public static void main(String[] args) {
    		long time1 = System.nanoTime(); // 시작 시간 읽기
    		
    		int sum = 0;
    		for(int i = 1; i <= 1000000; i++) {
    			sum += i;
    		}
    		
    		long time2 = System.nanoTime(); // 끝 시간 읽기
    		
    		System.out.println("1~1000000까지의 합 : " + sum);
    		System.out.println("계산에 " + (time2 - time1) + " 나노초 소요");
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/현재_시각_읽기(currentTimeMillis()_nanoTime())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판