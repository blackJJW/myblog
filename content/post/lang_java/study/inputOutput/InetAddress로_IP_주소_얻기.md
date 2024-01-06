---
title: "[Java] InetAddress로 IP 주소 얻기"
description: ""
date: "2023-01-17T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "Network"
---
<!--more-->

- 자바는 IP 주소를 `java.net.InetAddress` 객체로 표현
- InetAddress는 로컬 컴퓨터의 IP 주소뿐만 아니라 도메인 이름을 DNS에서 검색한 후 IP 주소를 가져오는 기능을 제공
- 로컬 컴퓨터의 InetAddress를 얻고 싶다면 `InetAddress.getLocalHost()` 메소드를 호출
    
    ```java
    InetAddress ia = InetAddress.getLocalHost();
    ```
    
- 외부 컴퓨터의 도메인 이름을 알고 있다면 두 개의 메소드를 사용하여 InetAddress 객체를 얻으면 된다.
    
    ```java
    InetAddress ia = InetAddress.getByName(String host);
    InetAddress[] iaArr = InetAddress.getAllByName(String host);
    ```
    
    - `getByName()` 메소드는 매개값으로 준 도메인 이름으로 DNS에서 단 하나의 IP 주소를 얻어와 InetAddress를 생성하고 리턴
    - 연결 클라이언트가 많은 회사의 경우 서버의 과부하를 막기 위해 하나의 도메인 이름에 여러 개의 컴퓨터 IP를 등록해서 운영
        - 이 경우 DNS에 등록된 모든 IP 주소를 얻고 싶다면 `getAllByName()` 메소드를 호출
        - 리턴 타입은 `InetAddress[]` 배열
- InetAddress 객체에서 IP 주소를 얻기 위해서는 `getHostAddress()` 메소드를 호출
    - 리턴값은 문자열로된 IP 주소
    
    ```java
    String ip = InetAddress.getHostAddress();
    ```
    
- ex) 로컬 컴퓨터의 IP와 네이버(www.naver.com)의 IP 정보를 출력
    - InetAddressEx.java
    
    ```java
    import java.net.InetAddress;
    import java.net.UnknownHostException;
    
    public class InetAddressEx {
    
    	public static void main(String[] args) {
    		try {
    			InetAddress local = InetAddress.getLocalHost();
    			System.out.println("my pc IP addr : " + local.getHostAddress());
    			
    			InetAddress[] iaArr = InetAddress.getAllByName("www.naver.com");
    			for(InetAddress remote : iaArr) {
    				System.out.println("www.naver.com IP addr : " + remote.getHostAddress());
    			}
    		} catch(UnknownHostException e) {
    			e.printStackTrace();
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/InetAddress로_IP_주소_얻기/Untitled.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판