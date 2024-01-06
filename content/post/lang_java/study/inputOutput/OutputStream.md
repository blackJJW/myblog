---
title: "[Java] OutputStream"
description: ""
date: "2022-12-30T16:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 바이트 기반 출력 스트림의 최상위 클래스로 추상 클래스
    - 모든 바이트 기반 출력 스트림 클래스는 이 클래스를 기반으로 만들어진다.
    - FileOutputStream, PrintStream, BufferedOutputStream, DataOutputStream 클래스는 모두 OutputStream 클래스를 상속
    
    ![Untitled](/images/lang_java/inputOutput/OutputStream/Untitled.png)
    
- OutputStream 클래스에는 모든 바이트 기반 출력 스트림이 기본적으로 가져야할 메소드가 정의되어 있다.
    
    ### OutputStream 클래스의 주요 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | void | write(int b) | 출력 스트림으로 1바이트를 보낸다.(b의 끝 1바이트) |
    | void | write(byte[] b) | 출력 스트림으로 주어진 바이트 배열 b의 모든 바이트를 보낸다. |
    | void | write(byte[] b,int off, int len) | 출력 스트림으로 주어진 바이트 배열 b[off]부터 len개까지의 바이트를 보낸다. |
    | void | flush() | 버퍼에 잔류하는 모든 바이트를 출력 |
    | void | close() | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다. |

## 1. write(int b) 메소드

- write(int b) 메소드는 매개 변수로 주어진 int 값에서 끝에 있는 1바이트만 출력 스트림으로 보낸다.
    - 매개변수가 int타입이므로 4바이트 모두를 보내는 것으로 오해할 수 있다.

![Untitled](/images/lang_java/inputOutput/OutputStream/Untitled%201.png)

```java
OutputStream os = new FileOutputStream("C:/test.txt");
byte[] data = "ABC".getBytes();
for(int i = 0; i < data.length; i++) {
	os.write(data[i]); // "A", "B", "C"를 하나씩 출력
}
```

## 2. write(byte[] b) 메소드

- `write(byte[] b)`는 매개값으로 주어진 바이트 배열의 모든 바이트를 출력 스트림으로 보낸다.

![Untitled](/images/lang_java/inputOutput/OutputStream/Untitled%202.png)

```java
OutputStream os = new FileOutputStream("C:/test.txt");
byte[] data = "ABC".getBytes();
os.write(data); // "ABC" 모두 출력
```

## 3. write(byte[] b, int off, int len) 메소드

- `write(byte[] b, int off, int len)`은 b[off]부터 len개의 바이트를 출력 스트림으로 보낸다.

![Untitled](/images/lang_java/inputOutput/OutputStream/Untitled%203.png)

```java
OutputStream os = new FileOutputStream("C:/test.txt");
byte[] data = "ABC".getBytes();
os.write(data, 1, 2); // "BC"만 출력
```

## 4. flush()와 close() 메소드

- 출력 스트림은 내부에 작은 버퍼(buffer)가 있어서 데이터가 출력되기 전에 버퍼에 쌓여있다가 순서대로 출력
- `flush()` 메소드는 버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 지우는 역할을 수행
    - 프로그램에서 더 이상 출력할 데이터가 없다면 `flush()` 메소드를 마지막으로 호출하여 버퍼에 잔류하는 데이터가 출력되도록 해야 한다.
- OutputStream을 더 이상 사용하지 않을 경우, `close()` 메소드를 호출해서 OutputStream에서 사용했던 시스템 자원을 풀어준다.

```java
OutputStream os = new FileOutputStream("C:/test.txt");
byte[] data = "ABC".getBytes();
os.write(data);
os.flush();
os.close();
```

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판