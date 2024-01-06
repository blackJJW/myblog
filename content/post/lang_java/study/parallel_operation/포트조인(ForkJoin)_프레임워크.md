---
title: "[Java] 병렬 처리, 포트조인(ForkJoin) 프레임워크"
description: ""
date: "2022-12-10T15:40:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- 병렬 스트림은 요소들을 병렬 처리하기 위해 포크조인(ForkJoin) 프레임워크(Framework)를 사용
- 병렬 스트림을 이용하면 런타임 시에 포크조인 프레임워크가 동작
    - 포크 단계에서는 전체 데이터를 서브 데이터로 분리
    - 서브 데이터를 멀티 코어에서 병렬로 처리
    - 조인 단계에서 서브 결과를 결합해서 최종 결과를 만들어 낸다.
    - ex) 쿼드 코어 CPU에서 병렬 스트림으로 작업을 처리할 경우
        - 스트림의 요소를 N개라고 보았을 경우
        - 포크 단계에서는 전체 요소를 4등분을 함
        - 1등분씩 개발 코어에서 처리하고 조인 단계에서는 3번의 결합 과정을 거쳐 최종 결과를 산출
    
    ![Untitled](/images/lang_java/parallel_operation/포트조인(ForkJoin)_프레임워크/Untitled.png)
    
    - 병렬 처리 스트림은 실제로 포크 단계에서 차례대로 요소를 4등분하지 않는다.
        - 내부적으로 서브 요소를 나누는 알고리즘이 있다.
- 포크조인 프레임워크는 포크와 조인 기능 이외의 스레드풀인 ForkJoinPool을 제공
- 각각의 코어에서 서브 요소를 처리하는 것은 개별 스레드가 해야 하므로 스레드 관리가 필요
- 포크조인 프레임워크는 ExecutorService의 구현 객체인 ForkJoinPool을 사용해서 작업 스레드를 관리
    
    ![Untitled](/images/lang_java/parallel_operation/포트조인(ForkJoin)_프레임워크/Untitled%201.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판