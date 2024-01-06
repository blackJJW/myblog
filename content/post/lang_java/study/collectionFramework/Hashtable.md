---
title: "[Java]  Map 컬렉션, Hashtable"
description: ""
date: "2022-11-13T15:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- HashMap과 동일한 내부 구조를 가지고 있다.
- Hashtable는 키로 사용할 객체는 `hashCode()`와 `equals()` 메소드를 재정의해서 동등 객체가 될 조건을 정해야 한다.
    
    ### HashMap과의 차이점
    
    - Hashtable은 동기화된(synchronized) 메소드로 구성되어 있기 때문에 멀티 스레드가 동시에 이 스레드가 동시에 이 메소드들을 실행 불가
        - 하나의 스레드가 실행을 완료해야만 다른 스레드 실행 가능
    - 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제 가능
        - 이것을 스레드가 안전(thread safe)하다라고 말한다.
    
    ![Untitled](/images/lang_java/collectionFramework/Map_컬렉션/Hashtable/Untitled.png)
    
- Hashtable의 생성 방법은 HashMap과 크게 다르지 않다.
    - 키 타입과 값 타입을 지정하고 기본 생성자를 호출하면 된다.
    
    ```java
    Map<K, V> map = new Hashtable<K, V>();
    // K : 키 타입
    // V : 값 타입
    ```
    
    - 키로 String 타입을 사용, 값으로 Integer 타입을 사용하는 Hashtable은 다음과 같이 생성
    
    ```java
    Map<String, Integer> map = new Hashtable<String, Integer>();
    ```
    
- ex) 아이디와 비밀번호 검사하기
    - 키보드로 아이디와 비밀번호를 입력받아서, Hashtable에 저장되어 있는 키와 값으로 비교한 후 로그인 여부를 출력
    - HashtableEx.java
    
    ```java
    import java.util.Hashtable;
    import java.util.Map;
    import java.util.Scanner;
    
    public class HashtableEx {
    
    	public static void main(String[] args) {
    		Map<String, String> map = new Hashtable<String, String>();
    		
    		// 아이디와 비밀번호를 미리 저장
    		map.put("spring", "12");
    		map.put("summer", "123");
    		map.put("fall", "1234");
    		map.put("winter", "12345");
    		
    		// 키보드로부터 입력된 내용을 받기 위해 생성
    		Scanner scanner = new Scanner(System.in);
    		
    		while(true) {
    			System.out.println("아이디와 비밀번호를 입력");
    			System.out.print("아이디 : ");
    			
    			// 키보드로 입력한 아이디를 읽는다.
    			String id = scanner.nextLine();
    			System.out.println();
    			
    			System.out.print("비밀번호 : ");
    			
    			// 키보드로 입력한 비밀번호를 읽는다.
    			String password = scanner.nextLine();
    			System.out.println();
    			
    			if(map.containsKey(id)) { // 아이디인 키가 존재하는지 확인
    				if(map.get(id).equals(password)) { // 비밀번호를 비교
    					System.out.println("로그인 됨");
    					break;
    				} else {
    					System.out.println("비밀번호가 일치하지 않음");
    				}
    			} else {
    				System.out.println("입력한 아이디가 존재하지 않음");
    			}
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/collectionFramework/Map_컬렉션/Hashtable/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판