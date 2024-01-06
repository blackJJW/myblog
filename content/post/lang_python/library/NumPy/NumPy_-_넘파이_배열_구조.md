---
title: "[Python] NumPy - 넘파이 배열 구조"
description: ""
date: "2023-02-19T17:00:45+09:00"
thumbnail: ""
categories:
  - "Python"
tags:
  - "Python"
  - "NumPy"

---

<!--more-->

### ChatGPT’s answer

> *NumPy is a powerful library for working with arrays and matrices in Python. It provides a high-performance multidimensional array object, as well as functions for performing mathematical operations on arrays.*
     
- 넘파이 모듈의 기본 배열은 다차원 배열인 `ndarray` 클래스의 객체
    - 이 배열은 하나의 자료형으로 만들어진 원소들을 보관하는 컨테이너
    - 다차원 배열의 객체가 만들어지면 데이터는 메모리에 넣고 이 데이터에 대한 다양한 메타 정보는 이 객체의 속성으로 관리

## 넘파이 배열 구조

- 넘파이 모듈을 사용하려면 numpy 모듈을 import 해야 한다.
    - 모듈의 별칭은 `as` 예약어 다음에 `np`로 지정
    - 모듈에 있는 `__version__` 변수로 현재 버전 확인
    
    ```python
    import numpy as np
    print(np.__version__)
    ```
    
    ![Untitled](/images/lang_python/library/NumPy/NumPy_-_넘파이_배열_구조/Untitled.png)
    
- 넘파이 모듈의 별칭과 `ndarray` 이름을 `print()`함수에 인자로 전달하면 클래스라는 것을 알 수 있다.
    
    ```python
    print(np.ndarray)
    ```
    
    ![Untitled](/images/lang_python/library/NumPy/NumPy_-_넘파이_배열_구조/Untitled%201.png)
    
    ```python
    print(np.ndarray.__name__)
    ```
    
    ![Untitled](/images/lang_python/library/NumPy/NumPy_-_넘파이_배열_구조/Untitled%202.png)
    
- 다차원 배열 클래스에 정의된 속성이나 메소드를 확인하기 위해 `namespace`를 확인
    - 내부의 `if`문을 사용해서 `ndarray` 클래스의 이름공간인 `__dict__` 에 문자열을 색인 검색
        - 객체를 가져와서 `var`와 동일하지 않는 객체만을 추출
    - 추출된 결과는 `ndarray` 클래스에서 관리하는 속성만 출력
    
    ```python
    for i in dir(np.ndarray):
        if not i.startswith("_"):
            if type(np.ndarray.__dict__[i]) != type(np.ndarray.var):
                print(i)
    ```
    
    ![Untitled](/images/lang_python/library/NumPy/NumPy_-_넘파이_배열_구조/Untitled%203.png)
    

---

### References

- ChatGPT
- 딥러닝 머신러닝을 위한 파이썬 넘파이 - 문용준, 문성혁 저, (주)잇플ITPLE 발행
- [NumPy Documentation](https://numpy.org/doc/)