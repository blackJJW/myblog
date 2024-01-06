---
title: "[Java] 스트림, 필터링(distict(), filter())"
description: ""
date: "2022-11-27T11:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 필터링은 중간 처리 기능으로 요소를 걸러내는 역할
- 필터링 메소드인 `distinct()`와 `filter()` 메소드는 모든 스트림이 가지고 있는 공통 메소드
    
    ![Untitled](/images/lang_java/stream/필터링(distict(),_filter())/Untitled.png)
    
- `distinct()` 메소드는 중복을 제거
    - Stream의 경우 `Object.equals(Object)`가 true이면 동일한 객체로 판단하고 중복을 제거
    - IntStream, LongStream, DoubleStream은 동일값일 경우 중복을 제거
    
    ![Untitled](/images/lang_java/stream/필터링(distict(),_filter())/Untitled%201.png)
    
- `filter()` 메소드는 매개값으로 주어진 Predicate가 true를 리턴하는 요소만 필터링
    
    ![Untitled](/images/lang_java/stream/필터링(distict(),_filter())/Untitled%202.png)
    
- ex) 필터링
    - 이름 List에서 중복된 이름을 제거하고 출력
    - 성이 “A”인 이름만 필터링해서 출력
    - FilteringEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    
    public class FilterEx {
    
    	public static void main(String[] args) {
    		List<String> names = Arrays.asList("ABC", "DEF", "GHI", "ABC", "A123", "ADE");
    		
    		names.stream()
    			.distinct() // 중복제거
    			.forEach(n -> System.out.println(n));
    		System.out.println();
    		
    		names.stream()
    			.filter(n -> n.startsWith("A")) // 필터링
    			.forEach(n -> System.out.println(n));
    		System.out.println();
    		
    		names.stream()
    			.distinct()  // 중복 제거 후 필터링
    			.filter(n -> n.startsWith("A"))
    			.forEach(n -> System.out.println(n));
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/필터링(distict(),_filter())/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판