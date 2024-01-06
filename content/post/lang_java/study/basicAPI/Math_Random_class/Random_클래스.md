---
title: "[Java] Random 클래스"
description: ""
date: "2022-10-01T10:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- `java.util.Random` 클래스는 난수를 얻어내기 위해 다양한 메소드를 제공
- `Math.random()` 메소드는 0.0에서 1 사이의 double 난수를 얻는 데만 사용
    - Random 클래스는 boolean, int, long, float, double 난수를 얻을 수 있다.
- Random 클래스느 시드값(seed)을 설정 가능
    - 시드값은 난수를 만드는 알고리즘에 사용되는 값으로 시드값이 같으면 같은 난수를 얻는다.
- Random 클래스로부터 Random 객체를 생성하는 방법
    
    
    | 생성자 | 설명 |
    | --- | --- |
    | Random() | 호출 시마다 다른 시드값(현재시간 이용)이 자동 설정 |
    | Random(long seed) | 매개값으로 주어진 시드값이 설정 |
- Random 클래스가 제공하는 메소드
    
    
    | 리턴값 | 메소드(매개 변수) | 설명 |
    | --- | --- | --- |
    | boolean | nextBoolean() | boolean 타입의 난수를 리턴 |
    | double  | nextDouble() | double 타입의 난수를 리턴(0.0 ≤ ~ < 1.0) |
    | int | nextInt() | int 타입의 난수를 리턴($-2^{31}$≤ ~ ≤ $2^{31}-1$) |
    | int | nextInt(int n) | int 타입의 난수를 리턴(0 ≤ ~ < n) |
- ex) 로또 번호 얻기
    - 로또의 여섯 숫자를 얻는 방법
    - 로또는 1 ~ 45 범위의 정수 숫자만 선택할 수 있으므로 `nextInt(45) + 1` 연산식을 사용
    - RandomEx.java
    
    ```java
    import java.util.Arrays;
    import java.util.Random;
    
    public class RandomEx {
    
    	public static void main(String[] args) {
    		// 선택 번호
    		int[] selectNumber = new int[6];
    		// 선택 번호 6개가 저장될 배열 생성
    		
    		// 선택 번호를 얻기 위한 Random 객체 생성
    		Random random = new Random(3);
    		System.out.print("선택 번호 : ");
    		for(int i = 0; i < 6; i++) {
    			// 선택 번호를 얻어 배열에 저장
    			selectNumber[i] = random.nextInt(45) + 1;
    			System.out.print(selectNumber[i] + " ");
    		}
    		System.out.println();
    		
    		// 당첨 번호
    		// 당첨 번호 6개가 저장될 배열 생성
    		int[] winningNumber = new int[6];
    		random = new Random(5); // 당첨 번호를 얻기 위한 Random 객체 생성
    		System.out.print("당첨 번호 : ");
    		for(int i = 0; i < 6; i++) {
    			// 선택 번호를 얻어 배열에 저장
    			winningNumber[i] = random.nextInt(45) + 1;
    			System.out.print(winningNumber[i] + " ");
    		}
    		System.out.println();
    		
    		// 당첨 여부
    		// 비교하기 전에 정렬
    		Arrays.sort(selectNumber);
    		Arrays.sort(winningNumber);
    		// 배열 항목 값 비교
    		boolean result = Arrays.equals(selectNumber, winningNumber);
    		System.out.print("당첨 여부 : ");
    		if(result) {
    			System.out.println("1등 당첨");
    		} else {
    			System.out.println("당첨 안됨");
    		}
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/Random_클래스/Untitled.png)
    
    - 선택 번호 6개를 얻기 위해 Random 객체를 생성할 때 시드값으로 3을 주었음
    - 당첨 번호 6개를 얻기 위해 Random 객체를 생성할 때 시드값으로 5를 주었음
        - 서로 다른 시드값을 주었기 때문에 선택 번호와 당첨 번호는 다를 수밖에 없다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판