---
title: "[Java] TCP 네트워킹, ServerSocket과 Socket의 용도"
description: ""
date: "2023-01-18T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "Network"
---
<!--more-->

- TCP 서버의 역할은 두 가지
    1. 클라이언트가 연결 요청을 해오면 연결을 수락
    2. 연결된 클라이언트와 통신
    - 자바에서는 이 두 역할별로 별도의 클래스를 제공
        - 클라이언트의 연결 요청을 기다리면서 연결 수락을 담당하는 `java.net.ServerSocket` 클래스
        - 연결된 클라이언트와 통신을 담당하는 `java.net.Socket` 클래스
- 클라이언트가 연결 요청을 해오면 ServerSocket은 연결을 수락하고 통신용 Socket을 만든다.
    
    ![Untitled](/images/lang_java/inputOutput/ServerSocket과_Socket의_용도/Untitled.png)
    
    - 서버는 클라이언트가 접속할 포트를 가지고 있어야 한다.
        - 이 포트를 바인딩(binding) 포트
        - 서버는 고정된 포트 번호에 바인딩해서 실행
        - ServerSocket을 생성할 때 포트 번호 하나를 지정해야 한다.
        - 위 그림에서는 5001번이 서버 바인딩 포트
    - 서버가 실행되면 클라이언트는 서버의 IP 주소와 바인딩 포트 번호로 Socket을 생성해서 연결 요청을 할 수 있다.
    - ServerSocket은 클라이언트가 연결 요청을 해오면 `accept()` 메소드로 연결 수락을 하고 통신용 Socket을 생성
    - 클라이언트와 서버는 각각 Socket을 이용해서 데이터를 주고 받게 된다.

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판