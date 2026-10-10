---
title: Personal
description: 여행, 회고, 일상
---

여행, 회고, 일상 등 개인적인 기록입니다.

```base
filters:
  and:
    - file.ext == "md"
    - file.inFolder("personal")
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
