---
title: "[Java] 메소드 재정의(@Override)"
description: ""
date: "2022-08-01T18:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 부모 클래스의 모든 메소드가 자식 클래스에게 맞게 설계되어 있다면 가장 이상적인 상속
- 어떤 메소드는 자식 클래스가 사용하기에 적합하지 않을 수가 있다.
    - 이 경우 상속된 일부 메소드는 자식 클래스에서 다시 수정해서 사용해야 한다.
    - 자바는 이런 경우에 메소드 오버라이딩(Overriding) 기능을 제공
- 메소드가 오버라이딩되었다면 부모 객체의 메소드는 숨겨지기 때문에, 자식 객체에서 메소드를 호출하면 오버라이딩된 자식 메소드가 호출
    
    ![Untitled](/images/lang_java/inheritance/메소드_재정의(@Override)/Untitled.png)
    
    ### 메소드 오버라이딩 규칙
    
    - 부모의 메소드와 동일한 시그니처(리턴 타입, 메소드 이름, 매개 변수 리스트)를 가져야 한다.
    - 접근 제한을 더 강하게 오버라이딩할 수 없다.
    - 새로운 예외(Exception)를 throws할 수 없다.
        
        ---
        
- 접근 제한을 더 강하게 오버라이딩할 수 없다는 것은 부모 메소드가 `public` 접근 제한을 가지고 있을 경우
    - 오버라이딩하는 자식 메소드는 `default`나 `private` 접근 제한으로 수정할 수 없다.
- 부모 메소드가 `default` 접근 제한을 가지면 재정의되는 자식 메소드는 `default` 또는 `public` 접근 제한을 가질 수 있다.
- ex) Carculator의 자식 클래스인 Computer에서 원의 넓이를 구하는 Calculator의 `areaCircle()` 메소드를 사용하지 않고, 좀 더 정확한 원의 넓이를 구하도록 오버라이딩
    - Caculator.java
        - 부모 클래스
        
        ```java
        public class Calculator {
        	double areaCircle(double r) {
        		System.out.println("Calculator 객체의 areaCircle() 실행");
        		return 3.14159 * r * r;
        	}
        }
        ```
        
    - Computer.java
        - 자식 클래스
        
        ```java
        public class Computer extends Calculator{
        	@Override
        	double areaCircle(double r) {
        		System.out.println("Computer 객체의 areaCircle() 실행");
        		return Math.PI * r * r;
        	}
        }
        ```
        
        - `@Override` 어노테이션은 생략 가능
            - 이 어노테이션을 붙여주게 되면 `areaCircle()` 메소드가 정확히 오버라이딩된 것인지 컴파일러가 체크하기 때문에 개발자의 실수를 줄여준다.
            - ex) `areaCircle()` → `areaCircl()`로 했을 경우
                - 컴파일 에러 발생
                
                ![Untitled](/images/lang_java/inheritance/메소드_재정의(@Override)/Untitled%201.png)
                
    - ComputerEx.java
        - 메소드 오버라이딩 테스트
        
        ```java
        public class ComputerEx {
        
        	public static void main(String[] args) {
        		int r = 10;
        		
        		Calculator calculator = new Calculator();
        		System.out.println("원면적 : " + calculator.areaCircle(r));
        		System.out.println();
        		
        		Computer computer = new Computer();
        		System.out.println("원면적 : " + computer.areaCircle(r)); 
                                              // 재정의된 메소드 호출
        	}
        
        }
        ```
        
        ![Untitled](/images/lang_java/inheritance/메소드_재정의(@Override)/Untitled%202.png)
        

### 오버라이딩 메소드 자동 생성 기능

- 부모 메소드의 시그니처를 정확히 모를 경우 매우 유용하게 사용
    1. 자식 클래스에서 오버라이딩 메소드를 작성할 위치로 입력 커서를 옮긴다.
        
        ![Untitled](/images/lang_java/inheritance/메소드_재정의(@Override)/Untitled%203.png)
        
    2. 메뉴에서 [ Source → Override/Implement Methods… ]를 선택
        
        ![Untitled](/images/lang_java/inheritance/메소드_재정의(@Override)/Untitled%204.png)
        
    3. 부모 클래스에서 오버라이딩될 메소드를 선택하고 OK 버튼을 클릭
        
        ![Untitled](/images/lang_java/inheritance/메소드_재정의(@Override)/Untitled%205.png)
        
        ![Untitled](/images/lang_java/inheritance/메소드_재정의(@Override)/Untitled%206.png)
        

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판