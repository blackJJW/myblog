---
title: "[Java] Writer"
description: ""
date: "2023-01-01T17:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 문자 기반 출력 스트림의 최상위 클래스로 추상 클래스
    - 모든 문자 기반 출력 스트림 클래스는 이 클래스를 상속받아서 만들어진다.
    - FileWriter, BufferedWriter, PrintWriter, OutputStreamWriter 클래스는 모두 Writer 클래스를 상속
    
    ![Untitled](/images/lang_java/inputOutput/Writer/Untitled.png)
    
    - Writer 클래스에는 모든 문자 기반 출력 스트림이 기본적으로 가져야 할 메소드가 정의되어 있다.
    
    ### Writer 클래스의 주요 메소드
    
    | 리턴 타입 | 메소드 | 설명 |
    | --- | --- | --- |
    | void | write(int c) | 출력 스트림으로 주어진 한 문자를 보낸다.(c의 끝 2바이트) |
    | void | write(char[] cbuf) | 출력 스트림으로 주어진 문자 배열 cbuf의 모든 문자를 보낸다. |
    | void | write(char[] cbuf, int off, int len) | 출력 스트림으로 주어진 문자 배열 cbuf[off]부터 len개까지의 문자를 보낸다. |
    | void | write(String str) | 출력 스트림으로 주어진 문자열을 전부 보낸다. |
    | void | write(String str, int off, int len) | 출력 스트림으로 주어진 문자열 off 순번부터 len개까지의 문자를 보낸다. |
    | void | flush() | 버퍼에 잔류하는 모든 문자열을 출력 |
    | void | close() | 사용한 시스템 자원을 반납하고 출력 스트림을 닫는다. |

## 1. write(int c) 메소드

- 매개 변수로 주어진 int값에서 끝에 있는 2바이트(한 개의 문자)만 출력 스트림으로 보낸다.
    - 매개 변수가 int타입이므로 4바이트 모두를 보내는 것으로 오해할 수 있다.

![Untitled](/images/lang_java/inputOutput/Writer/Untitled%201.png)

```java
Writer writer = new FileWriter("C:/test.txt");
char[] data = "ABC".toCharArray();
for(int i = 0; i < data.length; i++) {
	writer.write(data[i]); // "A", "B", "C"를 하나씩 출력
}
```

## 2. write(char[] cbuf) 메소드

- 매개값으로 주어진 `char[]` 배열의 모든 문자를 출력 스트림으로 보낸다.

![Untitled](/images/lang_java/inputOutput/Writer/Untitled%202.png)

```java
Writer writer = new FileWriter("C:/test.txt");
char[] data = "ABC".toCharArray();
writer.write(data); // "ABC" 모두 출력
```

## 3. write(char[] c, int off, int len) 메소드

- `c[off]`부터 len개의 문자를 출력스트림으로 보낸다.

![Untitled](/images/lang_java/inputOutput/Writer/Untitled%203.png)

```java
Writer writer = new FileWriter("C:/test.txt");
char[] data = "ABC".toCharArray();
writer.write(data, 1, 2); // "BC"만 출력
```

## 4. write(String str)와 write(String str, int off, int len) 메소드

- Writer는 문자열을 좀 더 쉽게 보내기 위해서 `write(String str)`와 `write(String str, int off, int len)` 메소드를 제공
    - `write(String str)` : 문자열 전체를 출력 스트림으로 보낸다.
    - `write(String str, int off, int len)` : 주어진 문자열 off 순번부터 len개까지의 문자를 보낸다.

![Untitled](/images/lang_java/inputOutput/Writer/Untitled%204.png)

![Untitled](/images/lang_java/inputOutput/Writer/Untitled%205.png)

- 문자 출력 스트림은 내부에 작은 버퍼(buffer)가 있어서 데이터가 출력되기 전에 버퍼가 쌓여있다가 순서대로 출력된다.

## 5. flush() 메소드

- 버퍼에 잔류하고 있는 데이터를 모두 출력시키고 버퍼를 비우는 역할 수행
- 프로그램에서 더 이상 출력할 문자가 없다면 `flush()` 메소드를 마지막으로 호출하여 모든 문자가 출력되도록 해야 한다.

## 6. close() 메소드

- Writer를 더 이상 사용하지 않을 경우에는 `close()` 메소드를 호출해서 Writer에서 사용했던 시스템 자원을 풀어준다.

```java
Writer writer = new FileWriter("C:/test.txt");
String data = "AB CD EFGH";
writer.write(data);
writer.flush();
writer.close();
```

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판