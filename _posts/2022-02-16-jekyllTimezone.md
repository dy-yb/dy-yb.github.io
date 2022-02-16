---
title: Jekyll Time zone setting
date: 2022-02-16 13:03:00
tags: gitblog study jekyll
---

gitblog 테마를 변경하고 난 이후로 정상적으로 빌드 되었음에도 불구하고 실제 블로그에는 나타나지 않았다. 
로컬에서 빌드하여 살펴보니 한국시간을 기준으로 date를 작성해서 이를 미래 시간으로 감지해 글을 게재하지 않도록 하는 것 같았다.

### jekyll time zone 변경하는 방법

(사용 테마: jekyll-Uno)

- _config.yml 파일에 다음 라인을 추가하고(위치는 상관 없음) 저장 후 빌드한다.
        ```bash
        timezone: Asia/Seoul
        ```
