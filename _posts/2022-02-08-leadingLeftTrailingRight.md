---
title: AutoLayout에서 leading/left, trailing/right의 차이
date: 2022-02-08 12:10:00
tags: xcode swift ios study
---

- leading, trailing: 글자가 시작 점 기준
- left, right: 사용자가 바라보는 뷰 기준
    
    → 다국어 지원 앱의 경우에 오른쪽에서 왼쪽으로 작성되는 언어가 있을 수 있는데, leading/trailingd으로 autolayout을 설정하면 반대위치에 label이 위치하게 될 수 있음.
