---
title: "[Java] InputStream"
description: ""
date: "2022-12-30T15:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 바이트 기반 입력 스트림의 최상위 클래스로 추상 클래스
- 모든 바이트 기반 입력 스트림은 이 클래스를 상속받아서 만들어진다.
    - FileInputStream, BufferedInputStream, DataInputStream 클래스는 모두 InputStream 클래스를 상속
    
    ![Untitled](/images/lang_java/inputOutput/InputStream/Untitled.png)
    
- InputStream 클래스에는 바이트 기반 입력 스트림이 기본적으로 가져야할 메소드가 정의
    
    ### InputStream 클래스의 주요 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | int | read() | 입력 스트림으로부터 1 바이트를 읽고 읽은 바이트를 리턴 |
    | int | read(byte[] b) | 입력 스트림으로부터 읽은 바이트들을 매개값으로 주어진 바이트 배열 b에 저장하고 실제로 읽은 바이트 수를 리턴 |
    | int | read(byte[] b, int off, int len) | 입력 스트림으로부터 len개의 바이트만큼 읽고 매개값으로 주어진 바이트 배열 b[off]부터 len개까지 지정. 그리고 실제로 읽은 바이트 수인 len개를 리턴. 만약 len개를 모두 읽지 못하면 실제로 읽은 바이트 수를 리턴. |
    | void | close() | 사용한 시스템 자원을 반납하고 입력 스트림을 닫는다. |

## 1. read() 메소드

- 입력 스트림으로부터 1바이트를 읽고 4바이트 int 타입으로 리턴
    - 리턴된 4바이트 중 끝의 1바이트에만 데이터가 들어가 있다.
    
    ex) 입력 스트림에서 5개의 바이트가 들어온다면 다음과 같이 `read()` 메소드로 1바이트씩 5번 읽을 수 있다.
    

![Untitled](/images/lang_java/inputOutput/InputStream/Untitled%201.png)

- 더 이상 입력 스트림으로부터 바이트를 읽을 수 없다면 `read()` 메소드는 -1을 리턴
    - 이것을 이용하면 읽을 수 있는 마지막 바이트까지 루프를 돌며 한 바이트식 읽을 수 있다.
    
    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    int readByte;
    while ((readByte = si.read()) != -1) { ... }
    ```
    

## 2. read(byte[] b) 메소드

- `read(byte[] b)` 메소드는 입력 스트림으로부터 매개값으로 주어진 바이트 배열의 길이만큼 바이트를 읽고 배열에 저장
    - 읽은 바이트 수를 리턴
    - 실제로 읽은 바이트 수가 배열의 길이보다 작을 경우 읽은 수만큼 리턴
    
    ex) 입력 스트림에서 5개의 바이트가 들어온다면 길이 3인 바이트 뱅열로 두 번 읽을 수 있다.
    

![Untitled](/images/lang_java/inputOutput/InputStream/Untitled%202.png)

- `read(byte[] b)` 역시 입력 스트림으로부터 바이트를 읽을 수 없다면 -1을 리턴
    - 이것을 이용하면 읽을 수 있는 마지막 바이트까지 루프를 돌며 읽을 수 있다.
    
    ```java
    InputStream is = new FileInputStream("C:/test.jpg");
    int readByteNo;
    byte[] readBytes = new byte[100];
    while ((readByteNo = is.read(readBytes)) != -1) { ... }
    ```
    
    - 입력 스트림으로부터 100개의 바이트가 들어온다면 `read()` 메소드는 100번을 루핑해서 읽어들어야 한다.
    - `read(byte[] b)` 메소드는 한 번 읽을 때 매개값으로 주어진 바이트 배열 길이만큼 읽기 때문에 루핑 횟수가 현저히 줄어든다.
    - 많은 양의 바이트를 읽을 때는 `read(byte[] b)` 메소드를 사용하는 것이 좋다.

## 3. read(byte[] b, int off, int len) 메소드

- `read(byte[] b, int off, int len)` 메소드 입력 스트림으로부터 len개의 바이트만큼 읽음
    - 매개값으로 주어진 바이트 배열 b[off]부터 len개까지 저장
    - 읽은 바이트 수인 len개를 리턴
    - 실제로 읽은 바이트 수가 len개보다 작을 경우 읽은 수만큼 리턴
    
    ex) 입력 스트림에서 전체 5개의 바이트가 들어오고, 여기서 3개만 읽어 b[2], b[3], b[4]에 각각 저장할 경우 다음과 같이 진행할 수 있다.
    

![Untitled](/images/lang_java/inputOutput/InputStream/Untitled%203.png)

- `read(byte[] b, int off, int len)`은 입력 스트림으로부터 바이트를 더 이상 읽을 수 없다면 -1을 리턴
- `read(byte[] b)`와의 차이점
    - 한 번에 읽어들이는 바이트 수를 len 매개값으로 조절 가능
    - 배열에서 저장이 시작되는 인덱스를 지정 가능
    - 만약 off를 0으로, len을 배열의 길이로 준다면 `read(byte[] b)`와 동일
    
    ```java
    InputStream is = ...;
    byte[] readBytes = new byte[100];
    int readByteNo = is.read(readBytes);
    ```
    
    ```java
    InputStream is = ...;
    byte[] readBytes = new byte[100];
    int readByteNo = is.read(readBytes, 0, 100);
    ```
    

## 4. close() 메소드

- InputStream을 더 이상 사용하지 않을 경우에는 `close()` 메소드를 호출해서 InputStream에서 사용했던 시스템 자원을 풀어준다.
    
    ```java
    is.close();
    ```
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판