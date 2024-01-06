---
title: "[Java] 콘솔 입출력, System.in 필드"
description: ""
date: "2023-01-05T21:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
---
<!--more-->

- 자바는 프로그램이 콘솔로부터 데이터를 입력받을 수 있도록 System 클래스의 in 정적 필드를 제공
- `System.in`은 IntStream 타입의 필드이므로 InputStream 변수로 참조가 가능
    
    ```java
    InputStream is = System.in;
    ```
    
- 키보드로부터 어떤 키가 입력되었는지 확인하려면 InputStream의 `read()` 메소드로 한 바이트를 읽으면 된다.\
    - 리턴된 int값에는 십진수 아스키 코드(Ascii Code)가 들어 있다.
    
    ```java
    int asciiCode = is.read();
    ```
    
    - 숫자로된 아스키 코드 대신에 키보드에서 입력한 문자를 직접 얻고 싶다면 `read()` 메소드로 읽은 아스키 코드를 char 타입 변환하면 된다.
    
    ```java
    char inputChar = (char) is.read();
    ```
    
    ex) `read()` 메소드로 읽은 아스키 코드 97번을 char 타입으로 변환하면 ‘a’ 문자를 얻을 수 있다.
    
    ```java
    char inputChar = (char) 97;
       // 'a'
    ```
    
- ex) 현금 자동 입출금기인 ATM(Automatic Teller Machine)과 비슷하게 사용자에게 메뉴를 제공
    - 사용자가 어떤 번호를 입력했는지 알아내는 예제
    - SystemInEx1.java
    
    ```java
    import java.io.InputStream;
    
    public class SystemInEx1 {
    
    	public static void main(String[] args) throws Exception {
    		System.out.println("== 메뉴 ==");
    		System.out.println("1. 메뉴 조회");
    		System.out.println("2. 예금 출금");
    		System.out.println("3. 예금 입금");
    		System.out.println("4. 종료 하기");
    		System.out.print("메뉴를 선택하세요");
    		
    		// 키보드 입력 스트림 얻기
    		InputStream is = System.in;
    		
    		// 아스키 코드를 읽고 문자로 리턴
    		char inputChar = (char) is.read();
    		
    		switch(inputChar) {
    			case '1':
    				System.out.println("예금 조회를 선택");
    				break;
    			case '2':
    				System.out.println("예금 출금을 선택");
    				break;
    			case '3':
    				System.out.println("예금 입금을 선택");
    				break;
    			case '4':
    				System.out.println("종료 하기를 선택");
    				break;
    		}
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/System_in_필드/Untitled.png)
    
- InputStream의 `read()` 메소드는 1바이트만 읽기 때문에 1바이트의 아스키 코드로 표현되는 숫자, 영어, 특수문자는 프로그램에서 잘 읽을 수 있다.
    - 한글과 같이 2바이트를 필요로 하는 유니코드는 `read()` 메소드로 읽을 수 없다.
- 키보드로 입력된 한글을 얻기 위해서는 우선 `read(byte[] b)`나 `read(byte[] b, int off, int len)` 메소드로 전체 입력된 내용을 바이트로 받음
    - 이 배열을 이용해서 String 객체를 생성
- `read(byte[] b)` 메소드를 사용하기 전에 우선 키보드에서 입력한 문자를 저장할 바이트 배열을 만들어야 한다.
- 바이트 배열의 길이는 읽어야 할 바이트 수를 고려해서 적절히 주면 된다.
    - 영어 한 문자는 1바이트, 한글 한 글자는 2바이트를 차지
    - 최대 영문자 15자 또는 한글 7자를 저장하려면 다음과 같이 바이트 배열을 선언
    
    ![Untitled](/images/lang_java/inputOutput/System_in_필드/Untitled%201.png)
    
- 생성된 배열을 `read(byte[] b)` 메소드의 매개값으로 주면 키보드에서 입력한 문자를 저장할 수 있게 된다.
    
    ```java
    byte[] byteData = new byte[15];
    int readByteNo = System.in.read(byteData);
      // 읽은 바이트 수           // 실제로 읽은 바이트 수 저장
    ```
    
- `read(byte[] b)` 메소드는 매개값으로 주어진 바이트 배열에 읽은 문자를 저장
    - 실제로 읽은 바이트 개수를 리턴
    
    ### read(byte[] b) 메소드가 동작하는 방법
    
    - 키보드에서 “Java”라고 입력하고 `Enter` 키를 누르면 바이트 배열에 저장되는 값과 리턴값
    
    ![Untitled](/images/lang_java/inputOutput/System_in_필드/Untitled%202.png)
    
- 프로그램에서 바이트 배열에 저장된 아스키 코드를 사용하려면 문자열로 변환해야 한다.
    - 변환할 문자열은 바이트 배열의 0번 인덱스에서 시작해서 읽은 바이트 수 - 2 만큼이다.
    - 2를 빼는 이유는 Enter키에 해당하는 마지막 두 바이트를 제거하기 위해서이다.
    - 바이트를 문자열로 변환할 때에는 String 클래스의 생성자를 이용
    
    ```java
    String strData = new String( byteData, 0, readByteNo - 2 );
                            // 바이트 배열    // 읽은 바이트 수 - 2
                                      // 시작 인덱스
    ```
    
- ex) 이름과 하고 싶은 말을 키보드로 입력받아 다시 출력하는 예
    - 콘솔에서 입력한 한글 밀어내기
    - SystemInEx2.java
    
    ```java
    import java.io.InputStream;
    
    public class SystemInEx2 {
    
    	public static void main(String[] args) throws Exception {
    		InputStream is = System.in;
    		
    		byte[] datas = new byte[100];
    		
    		System.out.print("이름 : ");
    		int nameBytes = is.read(datas);
    		String name = new String(datas, 0, nameBytes - 2);
    		
    		System.out.print("하고 싶은 말 : ");
    		int commentBytes = is.read(datas);
    		String comment = new String(datas, 0, commentBytes - 2);
    		
    		System.out.println("입력한 이름 : " + name);
    		System.out.println("입력한 하고 싶은 말 : " + comment);
    	}
    }
    ```
    
    ![Untitled](/images/lang_java/inputOutput/System_in_필드/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판