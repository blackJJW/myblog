---
title: "[Python] list"
description: ""
date: "2023-02-18T17:00:45+09:00"
thumbnail: ""
categories:
  - "Python"
tags:
  - "Python"


---
<!--more-->

- 리스트의 하나의 자료형은 파이썬에서 제공하는 최상위 클래스인 `object`
    - 파이썬으로 만들어지는 모든 클래스의 객체를 원소로 받을 수 있다.
    - 리스트 객체가 생성될 때 특정 자료형을 지정하지 않고 모든 객체를 받아 처리

### 배열 구조에서 원소 관리

- 원소를 저장하는 순서 : 인덱스(Index)
    - 내부 원소를 검색할 때 인덱스를 이용해서 수행

### 리스트에서 각 원소에 접근

- 리스트는 실제 원소의 객체를 저장하는 것이 아니라 객체의 레퍼런스를 관리
- 인덱스로 검색하면 객체의 레퍼런스를 사용해서 객체 정보를 반환

### 리스트 구조

- 대괄호( `[ ]` )를 사용해서 내부에 원소를 작성
- 리스트 내부에 원소를 넣고, 변수에 할당
    - 변수에 할당된 객체는 list 클래스
    
    ```python
    lst = [1, 2, 3, 4, 5]
    type(lst)
    ```
    
    ![Untitled](/images/lang_python/list/Untitled.png)
    

### 리스트 조회

- 색인 검색을 사용
    - 연산 기호도 대 괄호를 사용
    - 인덱스를 숫자로 지정
    
    ```python
    # 첫 번째 인덱스 : 0
    lst[0] 
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%201.png)
    
    ```python
    # 마지막 인덱스 : -1
    lst[-1]
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%202.png)
    
    ```python
    # 인덱스로 모든 원소 출력
    for i in range(len(lst)):
        print(lst[i], end = " ")
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%203.png)
    

### 2차원 리스트

- ex)
    
    ```python
    a = [[1, 2, 3], [4, 5, 6]]
    
    print(a[0])
    print(a[1])
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%204.png)
    
    ```python
    print(a[0][1])
    print(a[1][-1])
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%205.png)
    

### 리스트 조작

- `append()` : 원소 추가
    
    ```python
    lst = [1, 2, 3, 4, 5]
    
    lst.append(6)
    
    print(lst)
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%206.png)
    
- `insert(idx, value)` : 인덱스 위치에 값 삽입
    
    ```python
    lst.insert(0, 10)
    
    print(lst)
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%207.png)
    
- `remove(value)` : 원소 삭제
    
    ```python
    lst.remove(10)
    
    print(lst)
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%208.png)
    
- `len()` : 배열 길이
    
    ```python
    length = len(lst)
    print(length)
    ```
    
    ![Untitled](/images/lang_python/list/Untitled%209.png)
    

---

### References

- ChatGPT
- 딥러닝 머신러닝을 위한 파이썬 넘파이 - 문용준, 문성혁 저, (주)잇플ITPLE 발행
- [5. Data Structures](https://docs.python.org/3/tutorial/datastructures.html)