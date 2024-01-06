---
title: "[Java] JavaFX 스레드 동시성"
description: ""
date: "2022-12-28T15:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
---
<!--more-->

- JavaFX UI는 스레드에 안전하지 않기 때문에 UI를 생성하고 변경하는 작업은 JavaFX Application Thread가 담당
    - 다른 작업 스레드들은 UI를 생성하거나 변경할 수 없다.
- main 스레드가 Application의 `launch()` 메소드를 호출하면서 생성된 JavaFX Application Thread는 `start()` 메소드를 실행시키면서 모든 UI를 생성
- 컨트롤에서 이벤트가 발생할 경우 컨트롤러의 이벤트 처리 메소드를 실행하는 것도 JavaFX Application Thread
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_스레드_동시성/Untitled.png)
    
- JavaFX 애플리케이션을 개발할 때 주의할 점
    - JavaFX Application Thread가 시간을 요하는 작업을 하지 않도록 하는 것
    - 시간을 요하는 작업을 하게 되면 이 시간 동안에 UI는 반응하지 않고 멈춰있는 상태가 되기 때문에 다른 작업 스레드를 생성해서 처리하는 것이 좋다.
    - ex) 파일을 읽고 쓰거나, 네트워크상에서 데이터를 주고 받을 때 얼마만큼의 시간이 필요한 지 모르기 때문
        - 반드시 작업 스레드를 생성해서 처리해야 한다.
    - 문제는 작업 스레드가 직접 UI를 변경할 수 없다.
        - UI 변경이 필요한 경우 두 가지 방법으로 처리해야 한다.
            1. `Platform.runLater()` 메소드를 이용
            2. `javafx.concurrnet` API인 Task 또는 Service를 이용 

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판