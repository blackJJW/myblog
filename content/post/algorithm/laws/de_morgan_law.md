---
title: "[Algorithm][Laws] 드 모르간의 법칙"
description: ""
date: 2023-11-18T16:09:43Z
categories:
  - "Algorithm"
tags:
  - "Algorithm"

---
<!--more-->

각 조건을 부정하고 논리곱을 논리합으로, 논리합을 논리곱으로 바꾸고 다시 전체를 부정하면 원래의 조건과 같다.

- 이 법칙을 일반적으로 나타내면 다음과 같다.
	1. `x and y`와 `not(not x or not y)`의 논리값은 같다.
	2. `x or y`와 `not(not x and not y)`의 논리값은 같다.
	
e.g) 



```python
while True:
	if num >= 10 and num <= 99:
		break
```

```python
while True:
	if not(num < 10 or num > 99):
		break
```

- 위 의 두 코드의 논리 결과는 같다.