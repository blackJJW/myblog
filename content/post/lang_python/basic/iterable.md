---
title: "[Python] iterable 객체"
date: 2023-12-05T15:29:59Z
categories:
  - "Python"
tags:
  - "Python"
---
<!--more-->

`range()`함수는 iterable 객체를 생성한다.
```python
range(n) # 0 이상 n 미만인 수를 차례로 나열하는 수열

range(a, b) # a 이상 b 미만인 수를 차레로 나열하는 수열

range(a, b, step) # a 이상 b 미만인 수를 step 간격으로 나열하는 수열
```

이터러블 객체는 **반복할 수 있는 객체**를 말함

`for i in range(1, 5)`와 같이 `for ~ in`문에 사용할 수 있다.

- 파이썬에서 사용하는 대표적인 이터러블 자료형으로 list, str, tuple이 있다.


문자열, 리스트, 튜플, 집합, 딕셔너리 등의 자료형 객체는 모두 interable하다는 공통점이 있다.

iterable 객체는 원소를 하나씩 꺼내는 구조

iterable 객체를 내장 함수인 `iter()`의 인수로 전달하면 그 객체에 대한 iterator(반복자)를 반환한다.

### iterator
데이터의 나열을 표현하는 객체

iterator의 `__next__`함수를 호춣하거나 내장 함수인 `next()` 함수에 iterator를 전달하면 원소를 순차적으로 꺼낼 수 있다.

꺼낼 원소가 더 이상 없는 경우에는 `StopIteration`으로 예외 발생을 시킨다.