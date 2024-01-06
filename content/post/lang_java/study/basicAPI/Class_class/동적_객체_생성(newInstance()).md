---
title: "[Java] Class 클래스, 동적 객체 생성(newInstance())"
description: ""
date: "2022-09-24T17:30:45+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"


---
<!--more-->
# 동적 객체 생성(newInstance())

- Class 객체를 이용하면 new 연산자를 사용하지 않아도 동적으로 객체를 생성 가능
- 코드 작성 시에 클래스 이름을 결정할 수 없고, 런타임 시에 클래스 이름이 결정되는 경우에 매우 유용하게 사용된다.
- `Class.forName()` 메소드로 Class 객체를 얻은 다음 `newInstance()` 메소드를 호출하면 Object 타입의 객체를 얻을 수 있다.
    
    ```java
    try {
    	Class clazz = Class.forName("런타일 시 결정되는 클래스 이름");
    	Object obj = clazz.newInstance();
    } catch (ClassNotFoundException e) {
    } catch (InstantiationException e) {
    } catch (IllegalAccessException e) {
    }
    ```
    
    - `newInstance()` 메소드는 기본 생성자를 호출해서 객체를 생성하기 때문에 반드시 클래스에 기본 생성자가 존재해야 한다.
- 매개 변수가 있는 생성자를 호출하고 싶을 경우
    - 리플렉션으로 Constructor 객체를 얻어 `newInstance()` 메소드를 호출하면 된다.
- `newInstance()` 메소드는 두 가지 예외가 발생 가능
    - `InstantiationException` 예외는 해당 클래스가 추상 클래스이거나 인터페이스일 경우에 발생
    - `IllegalAccessException` 예외는 클래스나 생성자가 접근 제한자로 인해 접근할 수 없는 경우에 발생
    - 예외 처리 코드가 필요
- newInstance() 메소드의 리턴 타입은 Object
    - 이것을 원래 클래스 타입으로 변환해야만 메소드 사용 가능
    - 강제 타입 변환이 필요
        - 클래스 타입을 모르는 상태이므로 변환 불가
        - 이 문제에 경우 인터페이스 사용이 필요
    - ex) Action 인터페이스와 구현 클래스인 SendAction, ReceiveAction이 있다고 가정
        
        ![Untitled](/images/lang_java/basicAPI/동적_객체_생성(newInstance())/Untitled.png)
        
        - `Class.forName()` 메소드의 매개값으로 “SendAction” 또는 “ReceiverAction”을 주면 Class 객체가 만들어짐
        - Class 객체의 `newInstance()` 메소드로 Object 객체를 얻을 수 있음
        - 얻어진 객체는 모두 Action 인터페이스를 구현하고 있어 Action 인터페이스 타입으로 변환 가능
            - 그런 다음 Action 인터페이스의 `execute()` 메소드를 호출하면, 개별 클래스의 실체 메소드인 `execute()` 메소드가 실행된다.
        
        ```java
        Class clazz = Class.forName("SendAction" 또는 "ReceiveAction");
        Action action = (Action) clazz.newInstance();
        action.execute(); // SendAction 또는 ReceiveAction의 execute()가 실행됨
        ```
        
        - `Class.forName()` 메소드의 매개값으로 “SendAction” 문자열을 주었다면 SendAction 클래스의 `execute()` 메소드가 호출
        - 매개값으로 “ReceiveAction” 문자열을 주었다면 ReceiverAction 클래스의 `execute()` 메소드가 호출
        
        ![Untitled](/images/lang_java/basicAPI/동적_객체_생성(newInstance())/Untitled%201.png)
        
- ex)  동적 객체 생성 및 실행
    - 인터페이스
    - Action.java
    
    ```java
    public interface Action {
    	public void execute();
    }
    ```
    
    - 발신 클래스
    - SendAction.java
    
    ```java
    public class SendAction implements Action{
    	@Override
    	public void execute() {
    		System.out.println("sending data");
    	}
    }
    ```
    
    - 수신 클래스
    - ReceiveAction.java
    
    ```java
    public class ReceiveAction implements Action{
    	@Override
    	public void execute() {
    		System.out.println("receiving data");
    	}
    }
    ```
    
    - NewInstanceEx.java
    
    ```java
    public class NewInstanceEx {
    	public static void main(String[] args) {
    	
    		try {
    			Class clazz = Class.forName("basicAPI.system_class.SendAction");
    			//Class clazz = Class.forName("basicAPI.system_class.ReceiveAction");
    			Action action = (Action) clazz.newInstance();
    			action.execute();
    		} catch(ClassNotFoundException e) {
    			e.printStackTrace();
    		} catch(InstantiationException e) {
    			e.printStackTrace();
    		} catch(IllegalAccessException e) {
    			e.printStackTrace();
    		}
    	}
    
    }
    ```
    
    ![Untitled](/images/lang_java/basicAPI/동적_객체_생성(newInstance())/Untitled%202.png)
    
    ![Untitled](/images/lang_java/basicAPI/동적_객체_생성(newInstance())/Untitled%203.png)
    

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판