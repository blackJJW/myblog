---
title: "[Java] String 클래스, String 메소드"
description: ""
date: "2022-09-24T19:00:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->

- String은 문자열의 추출, 비교, 찾기, 분리, 변환 등과 같은 다양한 메소드를 가지고 있음.
    
    ### 사용 빈도수가 높은 메소드
    
    | 리턴 타입 | 메소드명(매개 변수 | 설명 |
    | --- | --- | --- |
    | char | charAt(int index) | 특정 위치의 문자 리턴 |
    | boolean | equals(Object anObject) | 두 문자열을 비교 |
    | byte[] | getBytes() | byte[]로 리턴 |
    | byte[] | getBytes(Charset charset) | 주어진 문자셋으로 인코딩한 byte[]로 리턴 |
    | int | indexOf(String str) | 문자열 내에서 주어진 문자열의 위치를 리턴 |
    | int | length() | 총 문자의 수를 리턴 |
    | String | replace(CharSequence target, CharSequence replacement) | target 부분을 replacement로 대치한 새로운 문자열을 리턴 |
    | String | substring(int beginIndex) | beginIndex 위치에서 끝까지 잘라낸 새로운 문자열을 리턴 |
    | String | substring(int beginIndex, int endIndex) | beginIndex 위치에서 endIndex 전까지 잘라낸 새로운 문자열을 리턴 |
    | String  | toLowerCase() | 알파벳 소문자로 변환한 새로운 문자열을 리턴 |
    | String | toUpperCase() | 알파벳 대무낮로 변환한 새로운 문자열을 리턴 |
    | String | trim() | 앞뒤 공백을 제거한 새로운 문자열을 리턴 |
    | String | valueOf(int i) | 기본 타입값을 문자열로 리턴 |
    | String | valueOf(double d) | 기본 타입값을 문자열로 리턴 |

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판