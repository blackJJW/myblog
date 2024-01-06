---
title: "[Java] 제네릭, 제한된 타입 파라미터(<T extends 최상위타입>)"
description: ""
date: "2022-10-29T12:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 타입 파라미터에 지정되는 구체적인 타입을 제한할 필요가 종종 있다.
    - ex) 숫자를 연산하는 제네릭 메소드는 매개값으로  Number 또는 하위 클래스 타입(Byte, Integer, Long, Double)의 인스턴스만 가져야 한다.
    - 이것이 제한된 타입 파라미터(bounded type parameter)가 필요한 이유
- 제한된 타입 파라미터를 선언하려면 타입 파라미터 뒤에 `extends` 키워드를 붙이고 상위 타입을 명시하면 된다.
    - 상위 타입은 클래스뿐만 아니라 인터페이스도 가능
    - 인터페이스라고 해서 `implements`를 사용하지 않는다.
    
    ```java
    public <T extends 상위타입> 리턴타입 메소드(매개변수, ...) { ... }
    ```
    
    - 타입 파라미터에 지정되는 구체적인 타입은 상위 타입이거나 상위 타입의 하위 또는 구현 클래스만 가능
    
    ### 주의할 점
    
    - 메소드의 중괄호`{}`안에서 타입 파라미터 변수로 사용 가능한 것은 사위 타입의 멤버(필드, 메소드)로 제한된다.
    - 하위 타입에만 있는 필드와 메소드는 사용 불가


- 숫자 타입만 구체적인 타입으로 갖는 제네릭 메소드 `compare()`
    
    ```java
    public <T extends Number> int compare(T t1, T t2) {
    	double v1 = t1.doubleValue();  // Number의 doubleValue() 메소드 사용
    	double v2 = t2.doubleValue();  // Number의 doubleValeu() 메소드 사용
    	return Double.comapre(v1, v2);
    }
    ```
    
    - `doubleValue()` 메소드는 Number 클래스에 정의되어 있는 메소드로 숫자를 double 타입으로 변환
    - `Double.compare()` 메소드는 첫 번째 매개값이 작으면 -1을, 같으면 0을, 크면 1을 리턴
- ex) 제네릭 메소드
    - Util.java
    
    ```java
    public class Util {
    	public static <T extends Number> int compare(T t1, T t2){
    		double v1 = t1.doubleValue();
    		double v2 = t2.doubleValue();
    		return Double.compare(v1, v2);
    	}
    }
    ```
    
    
- ex) 제네릭 메소드 호출
    - BoundTypeParameterEx.java
    
    ```java
    public class BoundTypeParameterEx {
    
    	public static void main(String[] args) {
    		// String str = Util.compare("a", "b"); (x)
    		//                  String은 Number 타입이 아님
    		
    		int result1 = Util.compare(10, 20);
    		                 // 20 : int -> Integer (자동 Boxing)
    		System.out.println(result1);
    		
    		int result2 = Util.compare(4.5, 3);
    		                 // 4.5 : double -> Double (자동 Boxing)
    		System.out.println(result2);
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/generic/제한된_타입_파라미터(_T_extends_최상위타입_)/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판