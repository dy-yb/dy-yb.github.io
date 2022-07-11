---
title: UICollectionView reload시 깜빡거리는 현상 없애기
date: 2022-07-11 22:30:01 +09:00
tags: iOS swift study optional collectionview
categories: [ iOS ]
---

UICollectionView의 reloadSection 메소드를 사용하여 변경 된 내용을 적용할 때, 자동으로 애니메이션 효과가 나면서 깜빡거리는 현상이 일어난다.

![화면 기록 2022-07-11 22 26 48 mov](https://user-images.githubusercontent.com/40792935/178276499-0fe41a28-8f04-434c-b1d6-5b260ce71707.gif)

이 때 performWithoutAnimation 메소드내에서 reload를 하면 깜빡거리는 현상을 해결할 수 있다.

### 적용 코드
```swift
UIView.performWithoutAnimation {
        self.containerView.tagCollectionView.reloadSections([0]) // 0번째 section만 reload
        self.containerView.tagCollectionView.scrollToItem( // 선택한 셀로 위치 이동
          at: index,
          at: .centeredHorizontally,
          animated: false)
      }
```

### 실행화면
![화면 기록 2022-07-11 22 27 14 mov](https://user-images.githubusercontent.com/40792935/178276539-d42204bf-fff3-4982-a5be-5c66babceca3.gif)





- 참고
  - [UiCollectionView reload without animation](https://www.notion.so/UICollectionView-reload-ea50022202fa4e238d7ba9114b1d87c3#70fdf0a06ac247a2b8fc82bb8c0a642f)
