---
title: "[Java] JavaFX 다이얼로그"
description: ""
date: "2022-12-25T20:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- 다이얼로그(Dialog)는 주 윈도우에서 알림 또는 사용자의 입력을 위해서 실행되는 서브 윈도우
- 다이얼로그는 자체적으로 실행 불가
    - 주 윈도우에 의해서 실행
    - 다이얼로그를 띄누는 주 윈도우를 다이얼로그의 소유자(owner) 윈도우라고 한다.
- 다이얼로그는 모달(modal)과 모달리스(modeless) 두 가지 종류가 있다.
    - 모달 다이얼로그 : 다이얼로그를 닫기 전까지 소유자 윈도우를 사용 불가
    - 모달리스 다이얼로그 : 소유자 윈도우를 계속 사용할 수 있다.
- JavaFX에서 제공하는 다이얼로그 종류
    - 파일을 선택하는 FileChooser
    - 디렉토리를 선택하는 DirectoryChooser
    - 팝업창을 뛰우는 Popup
    - 이 다이얼로그들은 `javafx.stage` 패키지에 모두 포함
- 다이얼로그도 윈도우이므로 Stage로 생성

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판