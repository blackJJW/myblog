---
title: "[Java] JavaFX, 컨테이너"
description: ""
date: "2022-12-15T22:00:30+09:00"
thumbnail: ""
categories:
  - "Java"
tags:
  - "Java"
  - "JavaFX"

---
<!--more-->

- 레이아웃을 작성할 때 컨트롤들을 쉽게 배치할 수 있도록 도와주는 클래스가 컨테이너
- `javafx.scene.layout` 패키지에는 다양한 컨테이너 클래스들이 존재
    - 접미사가 Pane으로 끝나는 클래스는 모두 컨테이너
    - 그 이외에 HBox, VBox
    
    | 컨테이너 | 설명 |
    | --- | --- |
    | AnchorPane | 컨트롤을 좌표로 배치하는 레이아웃 |
    | BorderPane | 위, 아래, 오른쪽, 왼쪽, 중앙에 컨트롤을 배치하는 레이아웃 |
    | FlowPane | 행으로 배치하되 공간이 부족하면 새로운 행에 배치하는 레이아웃 |
    | GridPane | 그리드로 배치하되 셀의 크기가 고정적이지 않은 레이아웃 |
    | StackPane | 컨트롤을 겹쳐서 배치하는 레이아웃 |
    | TilePane | 그리드로 배치하되 고정된 셀의 크기를 갖는 레이아웃 |
    | HBox | 수평으로 배치하는 레이아웃 |
    | VBox | 수직으로 배치하는 레이아웃 |

---

## References

- 이것이 자바다 신용권의 Java 프로그래밍 정복 - 신용권 지음, 한빛미디어 출판