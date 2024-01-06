---
title: "[Java] TCP 네트워킹"
description: ""
date: "2023-01-17T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "Network"
---
<!--more-->

- TCP(Transmission Control Protocol)는 연결 지향적 프로토콜
    - 연결 지향 프로토콜이란 클라이언트와 서버가 연결된 상태에서 데이터를 주고받는 프로토콜
    - 클라이언트가 연결 요청
        - 서버가 연결을 수락
            
            —> 통신 선로가 고정
            
        - 모든 데이터는 통신 선로를 통해 순차적으로 전달
    - TCP는 데이터를 정확하고 안정적으로 전달
- TCP의 단점
    - 데이터를 보내기 전에 반드시 연결이 형성(가장 시간이 많이 걸리는 작업)
    - 고정된 통신 선로가 최단선(네트워크 길이 측면)이 아닐 경우
        - 상대적으로 UDP(User Datagram Protocol)보다 데이터 전송 속도가 느릴 수 있다.
- 자바는 TCP 네트워킹을 위해 `java.net.ServerSocket`과 `java.net.Socket` 클래스를 제공

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판