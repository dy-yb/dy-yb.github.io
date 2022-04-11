---
title: 정규 표현식을 이용한 특정 키워드를 포함한 문장 삭제 방법
date: 2021-07-19 15:52:00
tags: regular_expression tips
categories: [ etc ]
---

회사에서 로그파일 분석 중 필요하여 찾아보았습니다. 추후에 유용히 사용할 수 있을 것 같아 간단히 기록을 남깁니다.

1. 사용하는 Text editer에서 찾아바꾸기(`Ctrl+H`) 실행
    - 저는 kwrite를 사용하였습니다. 에디터에 따라 표현식이 다를 수 있습니다.
2. Find에 정규표현식 입력
    - `^.*keyword.*`

        > 정규표현식 풀이: 
        **`^`**(문장 시작에)**`.0`**(사용 된 문자가 0개 이상이고)**`keyword`**(keyword를 포함하면서)**`.*`**(keyword 후에 문자가 0개 이상이고)**`$`**(문장이 끝남)
        - 만약 문장들이 개행을 포함할 경우에는 `\r` 이나 `\r\n` 을 붙여 마무리함(`^.*keyword.*\r\n`)
3. Replace는 비워두고 Replace all 실행
4. 지워진 부분에 생긴 빈 라인 삭제
    - Find에 정규 표현식 `[\n]` 입력

**Reference:** https://4urdev.tistory.com/35
