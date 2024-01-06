---
title: "[Python] JSON형식으로 변환"
description: ""
date: "2023-08-22T23:00:45+09:00"
thumbnail: ""
categories:
  - "Python"
tags:
  - "Python"
  - "Python Library"
  - "JSON"


---
<!--more-->

# 1. str 타입을 JSON형식으로 변환

- JSON형식으로 저장되어 있는 문자열을 JSON으로 변환
    
    ```python
    import json
    
    # 문자열
    str_data = '{"name": "A", "age": 30, "city": "Seoul"}'
    print(f"str_data type : {type(str_data)}")
    
    # JSON으로 변환
    json_data = json.loads(str_data)
    print(f"json_data : {type(json_data)}")
    ```
    
    ![Untitled](/images/lang_python/library/JSON/JSON형식으로_변환/Untitled.png)
    

# 2. `requests.model.Response` 객체를 JSON 형식으로 변환

- 응답 객체의 내용을 JSON 형식으로 파싱하여 Python 객체로 반환.
- HTTP 응답 데이터를 Python에서 사용할 수 있는 형태로 얻을 수 있다.
    
    ```python
    import requests
    import json
    
    # GET 요청으로 보내고 응답 객체를 받는다.
    response = requests.get('path-to-get')
    
    # 응답 객체의 내용을 JSON으로 파싱
    json_data = response.json()
    
    print(f"json_data type : {type(json_data)}")
    ```
    
    ![Untitled](/images/lang_python/library/JSON/JSON형식으로_변환/Untitled_1.png)