---
title: "[Java] NIO, Buffer의 위치 속성"
description: ""
date: "2023-01-24T12:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"

---
<!--more-->

- Buffer를 사용하려면 먼저 Buffer의 위치 속성 개념과 위치 속성이 언제 변경되는지 알고 있어야 한다.
    
    ### Buffer의 네 가지 위치 속성
    
    | 속성 | 설명 |
    | --- | --- |
    | position | 현재 읽거나 쓰는 위치값. 인덱스 값이기 때문에 0부터 시작. limit보다 큰 값을 가질 수 없다. 만약 position과 limit의 값이 같아진다면 더 이상 데이터를 쓰거나 읽을 수 없다는 뜻 |
    | limit | 버퍼에서 읽거나 쓸 수 있는 위치의 한계. 이 값은 capacity보다 작거나 같은 값을 가진다. 최초에 버퍼를 만들었을 때는 capacity와 같은 값을 가진다. |
    | capacity | 버퍼의 최대 데이터 개수(메모리 크기)를 나타냄. 인덱스 값이 아니라 수량이다. |
    | mark | reset() 메소드를 실행했을 때에 들어오는 위치를 지정하는 인덱스, mark() 메소드로 지정할 수 있다. 반드시 position 이하의 값으로 지정. position이나 limit의 값이 mark 값보다 작은 경우, mark는 자동 제거. mark가 없는 상태에서 reset() 메소드를 호출하면 InvaildMarkException이 발생. |
    - position, limit, capacity, mark 속성의 크기 관계
        - mark는 position보다 클 수 없다.
        - position은 limit보다 클 수 없다.
        - limit은 capacity보다 클 수 없다.
        
        ```
        0 ≤ mark ≤ position ≤ limit ≤ capacity
        ```
        
- ex) 7바이트 크기의 버퍼가 있다고 가정
    - 처음에는 limit과 capacity가 모두 7, position은 0
    - 버퍼의 크기가 7이므로 인덱스는 6까지
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled.png)
    
    - 먼저 2바이트를 버퍼에 저장
        - 2바이트는 position이 위치한 0 인덱스에서 시작해서 버퍼에 저장
        - 처음 2바이트는 채워지고 position은 2번 인덱스
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%201.png)
    
    - 계속해서 3바이트를 저장
        - 3바이트는 position 2 인덱스에서 시작해서 버퍼에 저장
        - 5바이트가 채워지고 position은 5번 인덱스
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%202.png)
    
    - 버퍼에 저장되어 있는 바이트를 읽을 경우
        - 먼저 `flip()` 메소드를 호출하면 limit을 현재 position 5 인덱스로 설정
        - position을 0번 인덱스로 설정
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%203.png)
    
    - 버퍼에서 3바이트를 읽는다고 가정
        - position이 0번 인덱스이므로 처음 3바이트가 읽혀진다.
        - position은 3번 인덱스로 이동
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%204.png)
    
    - position이 3번 인덱스를 가르키고 있을 때 `mark()` 메소드를 호출해서 현재 위치를 기억
        - `mark()`는 3번 인덱스에 위치
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%205.png)
    
    - 이어서 2바이트를 더 읽을 경우
        - position은 5번 인덱스로 이동
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%206.png)
    
    - position을 mark 위치로 다시 이동할 경우
        - `reset()` 메소드를 호출하면 position은 mark가 있는 3번 인덱스로 이동
        - 주의할 점은 mark가 없는 상태에서 `reset()` 메소드를 호출하면 InvalidMarkException 예외가 발생
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%207.png)
    
    - 버퍼를 되감아 동일한 데이터를 한 번 더 읽고 싶다고 가정
        - `rewind()` 메소드를 호출하면 limit은 변하지 않는다.
        - position은 0번 인덱스로 다시 설정
        - mark는 position이나 limit이 mark보다 작은 값으로 조정되면 자동적으로 없어진다.
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%208.png)
    
    - 만약 `rewind()` 대신 `clear()` 메소드를 호출하면 Buffer의 세 가지 속성은 초기화
        - limit은 capacity로, position은 0으로 설정
        - mark는 자동적으로 없어짐
        - 데이터는 삭제되지 않는다.
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%209.png)
    
- 버퍼의 위치 속성을 변경하는 또 다른 메소드로 `compact()`가 있다.
    - `compact()`를 호출하면 현재 position에서 limit 사이의 데이터가 0번 인덱스로 복사
        - 현재 position은 복사된 데이터 다음 위치로 이동
    
    ex) `flip()` 메소드 호출 후 세 개의 데이터를 읽고 position이 3번 위치에 있을 경우
    
    - `compact()`가 호출
        - 3번 인덱스와 4번 인덱스의 데이터
            - 0번 인덱스와 1번 인덱스로 복사
        - position은 2번 인덱스로 이동
        - limit은 capacity로 이동
        - 주의할 점은 0번과 1번 인덱스를 제외한 나머지 인덱스의 데이터는 삭제되지 않고 남아있다.
    
    ![Untitled](/images/lang_java/NIO/Buffer의_위치_속성/Untitled%2010.png)
    
    - `compact()`를 호출하는 이유는 읽지 않은 데이터 뒤에 새로운 데이터를 저장하기 위함

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판