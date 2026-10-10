---
title: Study
description: 공부하면서 정리한 학습 노트
---

C++, 알고리즘, 딥러닝, 로보틱스 등 공부하면서 정리한 노트입니다.

```base
filters:
  and:
    - file.ext == "md"
    - file.inFolder("study")
    - "!file.name.startsWith('index')"
properties:
  title:
    displayName: Note
  file.folder:
    displayName: Topic
  date:
    displayName: Date
views:
  - type: table
    name: All Notes
    groupBy:
      property: file.folder
      direction: ASC
    order:
      - title
      - date
    sort:
      - property: date
        direction: DESC
```
