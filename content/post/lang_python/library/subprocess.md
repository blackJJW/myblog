---
title: "[Python] subprocess"
description: ""
date: "2023-02-18T16:00:45+09:00"
thumbnail: ""
categories:
  - "Python"
tags:
  - "Python"
  - "Python Library"


---
<!--more-->

### ChatGPT’s answer

>*It allows you to spawn new processes, connect to their input/output/error pipes, and obtain their return codes.*

>*This module can be used for various purposes, such as running shell commands, launching external programs, and managing child processes.* 

>*It provides a more powerful alternative to the **`os.system()`** function, which is often used to run shell commands from within Python scripts.*

 
- subprocess는 여러 방법으로 시스템 명령어를 실행하는 모듈
- ex) 폴더 내 목록 출력
    
    ```python
    import subprocess
    
    result = subprocess.run(['cmd', '/c', 'dir'], 
                            stdout=subprocess.PIPE, 
                            encoding='cp949')
    # 리눅스의 경우 'ls', '-l' 로 실행
    
    print(result.stdout)
    ```
    
    - `run()` : 'cmd', '/c', 'dir' 명령어를 실행하기 위한 함수
    - `stdout=subprocess.PIPE` : 프로세스의 표준 출력을 캡처하도록 subprocess에 지시
    
    ![Untitled](/images/lang_python/library/subprocess/Untitled.png)
    
- ex) 실행 결과를 텍스트 파일에 저장
    
    ```python
    with open('output.txt', 'w') as f:
        subprocess.run(['cmd', '/c', 'dir'], stdout=f)
    ```
    
    ![Untitled](/images/lang_python/library/subprocess/Untitled%201.png)
    
- ex) python 파일 실행
    
    ```python
    import subprocess
    
    process = subprocess.Popen(['python', 'hello-world.py'], 
                               stdout=subprocess.PIPE)
    
    for line in process.stdout:
        print(line.decode(), end='')
    ```
    
    - `Popen()` : 파이썬 스크립트를 실행하는 새로운 Python 프로세스를 생성
    
    ![Untitled](/images/lang_python/library/subprocess/Untitled%202.png)
    

---

### References

- ChatGPT
- [NFL Player Contact Detection - Getting Started](https://www.kaggle.com/code/robikscube/nfl-player-contact-detection-getting-started)
    
- [069 시스템 명령어를 실행하려면? ― subprocess](https://wikidocs.net/124373)