---
title: Research
description: 논문 리뷰와 연구 노트
---

Motion Planning, End-to-End Driving 등 연구하면서 읽은 논문과 연구 노트입니다.

```base
filters:
  and:
    - file.ext == "md"
    - file.inFolder("research")
    - "!file.name.startsWith('index')"
properties:
  title:
    displayName: Note
  file.folder:
    displayName: Folder
  date:
    displayName: Date
views:
  - type: table
    name: All Notes
    order:
      - title
      - date
    sort:
      - property: date
        direction: DESC
```
