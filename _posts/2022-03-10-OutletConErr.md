---
title: Could not insert new outlet connection error
date: 2022-03-10 11:20:00 +0900
tags: swift study storyboard error
categories: [ iOS ]
---


Storyboard로 UI 작성 중에 다음 에러가 발생했다.

- Could not insert new outlet connection: Could not find any information for the class named

클래스 네임을 제대로 명시 한 것을 수 차례 확인 했는데도 같은 에러가 반복되어 해결방법을 찾아봤다. 정확한 원인은 알 수 없고 Xcode의 버그인 것 같다.


### 해결방법

1. Xcode 재시작
2. 클린빌드(cmd+Shift+K)
3. 다시 아울렛 변수 삽입 시도
4. 그래도 안된다면, Xcode 종료 
5. /Users/username/Library/Developer/Xcode/DerivedData 내에 프로젝트 이름을 가진 폴더를 삭제
6. Xcode 열어 빌드
7. 아마도 성공..!


끗..

### References
https://macinjune.com/all-posts/web-developing/swift/xcode-swift-not-insert-new-outlet-connection-해결/
https://stackoverflow.com/questions/29923881/could-not-insert-new-outlet-connection-could-not-find-any-information-for-the-c
