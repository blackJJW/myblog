---
title: "[Java] LIFO와 FIFO 컬렉션"
description: ""
date: "2022-11-19T21:30:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 후입 선출(LIFO : Last In First Out)은 나중에 넣은 객체가 먼저 빠져나가는 자료구조
- 선입 선출(FIFO : First In First Out)은 먼저 넣은 객체가 먼저 빠져나가는 자료구조
- 컬렉션 프레임워크에는 LIFO 자료구조를 제공하는 스택(Stack) 클래스와 FIFO 자료구조를 제공하는 큐(Queue) 인터페이스를 제공
    
    ### 스택과 큐의 구조
    
    ![Untitled](/images/lang_java/collectionFramework/LIFO와_FIFO_컬렉션/Untitled.png)
    
    - 스택(Stack)을 응용한 대표적인 예가 JVM 스택 메모리
        - 스택 메모리에 저장된 변수는 나중에 저장된 것부터 제거
    - 큐(Queue)를 응용한 대표적인 예가 스레드풀(ExecutorService)의 작업 큐
        - 작업 큐는 먼저 들어온 작업부터 처리

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판