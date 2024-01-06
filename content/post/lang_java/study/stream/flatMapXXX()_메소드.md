---
title: "[Java] 스트림, 매핑, flatMapXXX() 메소드"
description: ""
date: "2022-11-27T12:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `flatMapXXX()` 메소드는 요소를 대체하는 복수 개의 요소들로 구성된 새로운 스트림을 리턴
- 그림 예시
    - 스트림에서 A라는 요소는 A1, A2 요소로 대체된다고 가정
        - B라는 요소는 B1, B2로 대체된다고 가정
    - A1, A2, B1, B2 요소를 가지는 새로운 스트림이 생성
    
    ![Untitled](/images/lang_java/stream/flatMapXXX()_메소드/Untitled.png)
    
    ### `flatMapXXX()` 메소드의 종류
    
    | 리턴 타입 | 메소드(매개 변수) | 요소 -> 대체 요소 |
    | --- | --- | --- |
    | Stream<R> | flatMap(Function<T, Stream<R>>) | T -> Stream<R> |
    | DoubleStream | flatMap(DoubleFunction<DoubleStream>) | double -> DoubleStream |
    | IntStream | flatMap(IntFunction<IntStream>) | int -> IntStream |
    | LongStream | flatMap(LongFunction<LongStream>) | long -> LongStream |
    | DoubleStream | flatMapToDouble(Function<T, DoubleStream>) | T -> DoubleStream |
    | IntStream | flatMapToInt(Function<T, Stream>) | T -> IntStream |
    | LongStream | flatMapToLong(Function<T, LongStream>) | T -> LongStream |
- ex) 복수개의 요소로 대체
    - 입력된 데이터(요소)들이 `List<String>`에 저장되어 있다고 가정
        - 요소별로 단어를 뽑아 단어 스트림으로 재생성
    - 입력된 데이터들이 숫자라면 숫자를 뽑아 숫자 스트림으로 재생성
    - FlatMapEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.List;
    
    public class FlatMapEx {
    
    	public static void main(String[] args) {
    		List<String> inputList1 = Arrays.asList("java8 lambda", "stream mapping");
    		
    		inputList1.stream()
    			.flatMap(data -> Arrays.stream(data.split(" ")))
    			.forEach(word -> System.out.println(word));
    		
    		System.out.println();
    		
    		List<String> inputList2 = Arrays.asList("10, 20, 30", "40, 50, 60");
    		
    		inputList2.stream()
    			.flatMapToInt(data -> {
    				String[] strArr = data.split(",");
    				int[] intArr = new int[strArr.length];
    				for(int i = 0; i < strArr.length; i++) {
    					intArr[i] = Integer.parseInt(strArr[i].trim());
    				}
    				return Arrays.stream(intArr);
    			})
    			.forEach(number -> System.out.println(number));
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/stream/flatMapXXX()_메소드/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판