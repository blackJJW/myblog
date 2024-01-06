---
title: "[Java] Objects 클래스, 동등 비교(equals()와 deepEquals())"
description: ""
date: "2022-09-20T23:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `Objects.equals(Object a, Object b)`는 두 객체의 동등을 비교하는데 다음과 같은 결과를 리턴
    - a와 b가 모두 null일 경우 true를 리턴
    - a와 b가 null이 아닌 경우는 `a.equals(b)`의 결과를 리턴
    
    | a | b | Objects.equals(a, b) |
    | --- | --- | --- |
    | not null | not null | a.equals(b)의 리턴값 |
    | null | not null | false |
    | not null | null | false |
    | null | null | true |
- `Objects.deepEquals(Object a, Object b)` 는 두 객체의 동등을 비교
    - a와 b가 서로 다른 배열일 경우
        - 항목 값이 모두 같다면 true를 리턴
        - 이것은 `Arrays.deepEquals(Object[] a, Object[] b)`와 동일
    
    | a | b | Objects.deepEquals(a, b) |
    | --- | --- | --- |
    | not null (not array) | not null (not array) | a.equals(b)의 리턴값 |
    | not null (array) | not null (array) | Arrays.deepEquals(a, b)의 리턴값 |
    | not null | null | false |
    | null | not null | false |
    | null | null | true |
- ex) 객체 동등 비교
    - EqualsAndDeepEqualsEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.Objects;
    
    public class EqualsAndDeepEqualsEx {
    
    	public static void main(String[] args) {
    		Integer o1 = 1000;
    		Integer o2 = 1000;
    		System.out.println(Objects.equals(o1, o2));
    		System.out.println(Objects.equals(o1, null));
    		System.out.println(Objects.equals(null, o2));
    		System.out.println(Objects.equals(null, null));
    		System.out.println(Objects.deepEquals(o1, o2) + "\n");
    		
    		Integer[] arr1 = {1, 2};
    		Integer[] arr2 = {1, 2};
    		System.out.println(Objects.equals(arr1, arr2));
    		System.out.println(Objects.deepEquals(arr1, arr2));
    		System.out.println(Arrays.deepEquals(arr1, arr2));
    		System.out.println(Objects.deepEquals(null, arr2));
    		System.out.println(Objects.deepEquals(arr1, null));
    		System.out.println(Objects.deepEquals(null, null));
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/동등_비교(equals()와_deepEquals())/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판