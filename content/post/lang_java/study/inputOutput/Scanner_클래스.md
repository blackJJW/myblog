---
title: "[Java] 콘솔 입출력, Scanner 클래스"
description: ""
date: "2023-01-07T13:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- Console 클래스는 콘솔로부터 문자열은 읽을 수 있지만 기본 타입(정수, 실수) 값을 바로 읽을 수는 없다.
- `java.io` 패키지의 클래스는 아니지만, `java.util` 패키지의 Scanner 클래스를 이용하면 콘솔로부터 기본 타입의 값을 바로 읽을 수 있다.
- Scanner 객체를 생성하려면 생성자에 `System.in` 매개값을 주면된다.
    
    ```java
    Scanner scanner = new Scanner(System.in);
    ```
    
- Scanner가 콘솔에서만 사용되는 것은 아니고, 생성자 매개값에는 File, InputStream, Path 등과 같이 다양한 입력 소스를 지정할 수도 있다.
    
    ### Scanner 기본 타입의 값을 읽기 위해 메소드를 제공
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | boolean | nextBoolean() | boolean(true/false) 값을 읽는다. |
    | byte | nextByte() | byte 값을 읽는다. |
    | short | nextShort() | short 값을 읽는다. |
    | int | nextInt() | int 값을 읽는다. |
    | long | nextLong() | long 값을 읽는다. |
    | float | nextFloat() | float 값을 읽는다. |
    | double | nextDouble | double 값을 읽는다. |
    | String | nextLine() | String 값을 읽는다. |
    - 위 메소드들은 콘솔에서 데이터를 입력한 후 `Enter` 키를 누르면 동작하도록 되어 있다.
- ex) 콘솔로부터 문자열, 정수, 실수를 직접 읽고 다시 콘솔로 출력
    - ScannerEx.java
    
    ```java
    import java.util.Scanner;
    
    public class ScannerEx {
    
    	public static void main(String[] args) {
    		Scanner scanner = new Scanner(System.in);
    		
    		System.out.print("문자열 입력 > ");
    		String inputString = scanner.nextLine();
    		System.out.println(inputString);
    		System.out.println();
    		
    		System.out.print("정수 입력 > ");
    		int inputInt = scanner.nextInt();
    		System.out.println(inputInt);
    		System.out.println();
    		
    		System.out.print("실수 입력 > ");
    		double inputDouble = scanner.nextDouble();
    		System.out.println(inputDouble);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/Scanner_클래스/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판