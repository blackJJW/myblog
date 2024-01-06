---
title: "[Java] Wrapper(포장) 클래스"
description: ""
date: "2022-09-29T20:23:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->


- 자바는 기본 타입(byte, char, short, int, long, float, double, boolean)의 값을 갖는 객체를 생성 가능
    - 이런 객체를 포장(Wrapper) 객체
    - 기본 타입의 값을 내부에 두고 포장하기 때문
- 포장 객체의 특징은 포장하고 있는 기본 타입 값은 외부에서 변경할 수 없다.
    - 내부의 값을 변경하고 싶다면 새로운 포장 객체를 만들어야 한다.
    
    ![Untitled](/images/lang_java/basicAPI/Wrapper(포장)_클래스/Untitled.png)
    
- 포장 클래스는 `java.lang` 패키지에 포함
    - 다음과 같이 기본 타입에 대응되는 클래스들이 있다.
    - char 타입과 int 타입이 각각 Character와 Integer로 변경
    - 기본 타입의 첫 문자를 대문자로 바꾼 이름
    
    | 기본 타입 | 포장 클래스 |
    | --- | --- |
    | byte | Byte |
    | char | Character |
    | short | Short |
    | int | Integer |
    | long | Long |
    | float | Float |
    | double | Double |
    | boolean | Boolean |

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판