---
title: "[Java] 람다식, andThen()과 compose() 디폴트 메소드"
description: ""
date: "2022-11-05T12:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 디폴트 및 정적 메소드는 추상 메소드가 아니기 때문에 함수적 인터페이스에 선언되어고 여전히 함수적 인터페이스의 성질을 잃지 않는다.
- 함수적 성질이란 하나의 추상 메소드를 가지고 있고, 람다식으로 익명 구현 객체를 생성할 수 있는 것을 말한다.
- `java.util.function` 패키지의 함수적 인터페이스는 하나 이상의 티폴트 및 정적 메소드를 가지고 있다.
- Consumer, Function, Operator 종류의 함수적 인터페이스는 `andThen()`과 `compose()` 디폴트 메소드를 가지고 있다.
    - `andThen()`과 `compose()` 디폴트 메소드는 두 개의 함수적 인터페이스를 순차적으로 연결
    - 첫 번째 처리 결과를 두 번째 매개값으로 제공해서 최종 결과값을 얻을 때 사용
    - `andThen()`과 `compose()`의 차이점은 어떤 함수적 인터페이스부터 먼저 처리하느냐이다.
    
    ### `andThen()`
    
    ```java
    인터페이스AB = 인터페이스A.andThen(인터페이스B);
    최종결과 = 인터페이스AB.method();
    ```
    
    - 인터페이스AB의 `method()`를 호출하면 우선 인터페이스A부터 처리하고 결과를 인터페이스B의 매개값으로 제공
    - 인터페이스B는 제공받은 매개값을 가지고 처리한 후 최종 결과를 리턴
    
    ![Untitled](/images/lang_java/lambda/andThen()과_compose()_디폴트_메소드/Untitled.png)
    
    ### `compose()`
    
    - 인터페이스AB의 method()를 호출하면 우선 인터페이스B부터 처리하고 결과를 인터페이스A의 매개값으로 제공
    - 인터페이스A는 제공받은 매개값을 가지고 처리한 후 최종 결과를 리턴
    
    ```java
    인터페이스AB = 인터페이스A.compare(인터페이스B);
    최종결과 = 인터페이스AB.method();
    ```
    
    ![Untitled](/images/lang_java/lambda/andThen()과_compose()_디폴트_메소드/Untitled%201.png)
    
- `andThen()`과 `compose()` 디폴트 메소드를 제공하는 `java.util.function` 패키지의 함수적 인터페이스
    
    ### Consumer
    
    | 함수적 인터페이스 | andThen() | compose()  |
    | --- |:----------:|:---:|
    | Consumer<T> | ○ |            |
    | BiConsumer<T, U> | ○ |            |
    | DoubleConsumer | ○ |            |
    | IntConsumer | ○ |            |
    | LongConsumer | ○ |            |
    
    ### Function
    
    | 함수적 인터페이스 | andThen() | compose() |
    | --- |:---------:|:---:|
    | Function<T, R> | ○ |     ○     |
    | BiFunction<T, U, R> | ○ |           |
    
    ### Operator
    
    | 함수적 인터페이스 | andThen() | compose()  |
    | --- |:----------:|:---:|
    | BinaryOperator<T> | ○ |            |
    | DoubleUnaryOperator | ○ |     ○      |
    | IntUnaryOperator | ○ |     ○      |
    | LongUnaryOperator | ○ |     ○      |

---

## Consumer의 순차적 연결

- consumer 종류의 함수적 인터페이스는 처리 결과를 리턴하지 않기 때문에 `andThen()` 디폴트 메소드는 함수적 인터페이스의 호출 순서만 정함
- ex) Consumer의 순차적 연결
    - `Consumer<Member>` 함수적 인터페이스 두 개를 순차적으로 연결해서 실행
    - 첫 번째 `Consumer<Member>`는 이름을 출력
    - 두 번째 `Consumer<Member>`는 아이디를 출력
    - ConsumerAndThenEx.java
    
    ```java
    import java.util.function.Consumer;
    
    public class ConsumerAndThenEx {
    
    	public static void main(String[] args) {
    		Consumer<Member> consumerA = (m) -> {
    			System.out.println("consumerA : " + m.getName());
    		};
    		
    		Consumer<Member> consumerB = (m) -> {
    			System.out.println("consumerB : " + m.getId());
    		};
    		
    		Consumer<Member> consumerAB = consumerA.andThen(consumerB);
    		
    		consumerAB.accept(new Member("ABC", "A", null));
    
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/andThen()과_compose()_디폴트_메소드/Untitled%202.png)
    
- ex) 회원 클래스
    - Member.java
    
    ```java
    public class Member {
    	private String name;
    	private String id;
    	private Address address;
    
    	public Member(String name, String id, Address address) {
    		this.name = name;
    		this.id = id;
    		this.address = address;
    	}
    	
    	public String getName() { return name; }
    	public String getId() { return id; }
    	public Address getAddress() { return address; }
    	
    }
    ```
    
- ex) 주소 클래스
    - Address.java
    
    ```java
    public class Address {
    	private String country;
    	private String city;
    	
    	public Address(String country, String city) {
    		this.country = country;
    		this.city = city;
    	}
    	
    	public String getCountry() { return country; }
    	public String getCity() { return city; }
    }
    ```
    

---

## Function의 순차적 연결

- Function과 Operator 종류의 함수적 인터페이스는 먼저 실행한 함수적 인터페이스의 결과를 다음 함수적 인터페이스의 매개값으로 넘겨주고, 최종 처리 결과를 리턴
- ex) `Function<Member, Address>`와 `Function<Address, String>`을 순차적으로 연결해서 `Function<Member, String>`을 생성한다고 가정
    - `Function<Member, Address>`는 매개값으로 제공되는 Member로부터 Address를 리턴
    - `Function<Address, String>`은 매개값으로 제공되는 Address로부터 String 리턴
    - 이 둘을 `andThen()`이나 `compose()`로 연결할 경우
        - `Function<Member, Address>`에서 리턴한 Address를 `Function<Address, String>`의 매개값으로 넘겨서 최종 String 타입을 리턴하는 `Function<Member, String>`을 생성
    
    ![Untitled](/images/lang_java/lambda/andThen()과_compose()_디폴트_메소드/Untitled%203.png)
    
    - Address는 두 함수적 인터페이스 간의 전달 데이터
        - Address는 내부적으로 전달되기 때문에 최종 함수적 인터페이스의 형태는 입력 데이터가 Member, 출력 데이터가 String이 되는 `Function<Member, String>`이 된다.
- ex) Function의 순차적 연결
    - Member 객체의 필드인 Address에서 city 정보를 얻어내기 위해 두 Function 함수적 인터페이스를 `andThen()`과 `compose()`를 이용해서 순차적으로 연결
    - FunctionAndThenComposeEx.java
    
    ```java
    import java.util.function.Function;
    
    public class FunctionAndThenComposeEx {
    
    	public static void main(String[] args) {
    		Function<Member, Address> functionA;
    		Function<Address, String> functionB;
    		Function<Member, String> functionAB;
    		String city;
    		
    		functionA = (m) -> m.getAddress();
    		functionB = (a) -> a.getCity();
    		
    		functionAB = functionA.andThen(functionB);
    		city = functionAB.apply(
    				new Member("ABC", "A", new Address("Korea", "Seoul"))
    				);
    		System.out.println("city : " + city);
    		
    		functionAB = functionB.compose(functionA);
    		city = functionAB.apply(
    				new Member("ABC", "A", new Address("Korea", "Seoul"))
    				);
    		System.out.println("city : " + city);
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/lambda/andThen()과_compose()_디폴트_메소드/Untitled%204.png)
    
    - `andThen()` 메소드를 호출한 함수적 인터페이스는 functionA
    - `compose()` 메소드를 호출한 함수적 인터페이스는 functionB
    - 모두 functionA부터 실행하고 functionB를 나중에 실행

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판