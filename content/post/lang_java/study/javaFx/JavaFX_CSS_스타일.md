---
title: "[Java] JavaFX CSS 스타일"
description: ""
date: "2022-12-26T15:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"
  - "CSS"
---
<!--more-->

- JavaFX UI를 담당하는 컨테이너 및 컨트롤은 CSS(Cascading Style Sheets)를 적용해서 모양 및 색상 등을 변경 가능
    - HTML에 CSS를 적용하는 것과 유사
    
    ![Untitled](/images/lang_java/javaFx/JavaFX_CSS_스타일/Untitled.png)
    
- JavaFX CSS 속성명은 CSS 속성명 앞에 “`-fx-`”가 더 붙는다.
- 기본 CSS 대신 다른 모양과 색상을 주고 싶다면 커스텀 CSS를 정의하면 된다.
    - 커스텀 CSS는 기본 CSS를 오버라이딩해서 기본 속성을 변경하거나, 새로운 속성을 정의한 것을 말한다.
- 커스텀 CSS를 적용하는 방법은 두 가지가 있다.
    - 인라인(inline) 스타일로 컨테이너 또는 컨트롤의 style 속성을 이용해서 CSS를 직접 적용
    - 외부 CSS 파일을 생성하고 Scene에 적용하는 것

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판
- [https://docs.oracle.com/javafx](https://docs.oracle.com/javafx)