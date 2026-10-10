---
title: Projects
description: 진행했거나 진행 중인 프로젝트
---

F1Tenth, AILAB 인턴십, MIRU 등 직접 참여한 프로젝트 기록입니다.

```base
filters:
  and:
    - file.ext == "md"
    - file.inFolder("projects")
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
