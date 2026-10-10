---
title: Guides
description: 따라 하면 되는 환경 설정 가이드
---

Ubuntu 터미널, ZSH, Git 등 개발 환경 설정 가이드입니다.

```base
filters:
  and:
    - file.ext == "md"
    - file.inFolder("guides")
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
