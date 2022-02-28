---
title: 기기 연결 빌드 시 오류 해결 방법
date: 2022-02-28 13:00:00 +0900
tags: swift study xcode
---

가끔 시뮬레이터가 아닌 실제 기기에 연결시켜 보아야만 제대로 작동하는지 알 수있는 기능들이 있어서 연결하여 확인하곤 했는데, 오랜만에 내 아이폰으로 빌드를 시도했더니


**iPhone is busy: Making Apple Watch ready for development**


이런 오류가 나타나면서 계~~속 빌드가 실패했다. 최근에 애플워치를 연동하면서 설정이 뭔가 꼬이게 된 것이라 추측 되었고,, 해당 에러로 검색을 해보니 이미 꽤나 많은 사람들이 애를 먹었나 보더라,, (무려 19k view!)

[Apple Developer Forums 링크](https://developer.apple.com/forums/thread/691452?page=3)

그런데 해결방법 상태가..😂

1. 아이폰을 재부팅 해보세요
2. 기기 연결을 해지하고 애플워치 블루투스를 끄고 다시 연결해보세요
3. 기기 연결 해지하고 애플워치 블투 끄고 폰도 끄고 다시해보세요
4. 비행기모드 해보세요
5. 등록된 기기를 삭제하고 이 모든 작업을 다시 반복해 보세요~
6. ... 

더 문제는 저거라도 해보자하는 심정으로 정말 안해본 방법이 없는데 다 안됐다.. 이 것 때문에 핸드폰과 워치 페어링을 아예 끊어버릴 수도 없으니.. 

그런데 워치 블루투스, 와이파이, 셀룰러 모조리 끄고 전원도 끄고 5분정도가 지나고 나니까 그제야 완전히 페어링이 끊긴건지 그제야 다른 에러가 등장!(ㅠㅠ)


**Unable to prepare device for development. Please check tthe connection to the device, and review all errors in the Device and Simulators window**


이 에러는 xcode와 iOS의 버전이 맞지 않을 때 나타난다고 한다.. 그래서 xcode를 13.0에서 13.2로 업데이트했고!(약 1시간 소요ㅎ) 에러는 박멸됐다..

더 신기한 점은 한번 페어링이 제대로 되고 나니까 애플워치가 연결이 되어있어도 문제없이 빌드가 잘 된다는 것이다.. ^^;

끝!
