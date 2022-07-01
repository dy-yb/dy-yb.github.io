---
title: UICollectionViewCell 재사용 시 데이터 중첩 문제 해결방법
date: 2022-07–01 13:10:00 +09:00
tags: iOS swift study collectionView
categories: [ iOS ]
---
# collection view cell 재사용 중복

ImageView를 가진 CollectionView를 만들 던 중에 Cell이 재사용 되면서 발생하는 듯한 문제가 나타났습니다.

빌드 후 시뮬레이터에서 CollectionView를 스크롤하여 아래로 내리다 보면(특히 빠른 속도로 스크롤 하는 경우),  해당 셀에 맞는 데이터가 아닌 이전 셀의 데이터가 중첩되어 나타났습니다.

- 실행 화면

<img width="426" alt="스크린샷_2022-06-30_22 52 34" src="https://user-images.githubusercontent.com/40792935/176822129-ddfd337b-2b91-4f28-b86d-1486589fe5d3.png">

### PrepareForReuse()

해당 문제는 셀이 재사용 될 때 이전 데이터도 함께 재사용 하기 때문에 발생되게 되는데, 이 때 이러한 문제를 해결 할 수 있는 것이 `CollectionViewCell`의 `prepareForReuse()` 함수 입니다. `TableView`에서도 활용 할 수 있습니다.

![Untitled](https://user-images.githubusercontent.com/40792935/176822130-5020b743-0584-418c-8c2a-25e9f9f136be.png)

사진은 tableView의 흐름도이지만 CollectionView도 동일하게 생각하면 됩니다. `cellForRowAt` 내의 코드가 실행되기 이전에 cell을 준비하는 단계라고 볼 수 있습니다. 따라서 그 때 이전 데이터들을 초기화 시켜주는 것으로 문제를 해결 할 수 있습니다.

저는 ScrollView를 이용하여 이미지 슬라이드를 만들었기 때문에, scrollView의 content를 초기화 시켜주었습니다. 그리고 상태에 따라 나타나는 ui가 있는 경우에도 해당 함수에서 초기화를 해주면 조건에 맞지 않은 곳에서 갑자기 나타나는 현상을 방지 할 수 있습니다.

- 적용 코드

```swift
class CollectionViewCell: UICollectionViewCell {
...

override func prepareForReuse() {
    super.prepareForReuse()
    self.imageScrollView.subviews.forEach{
      $0.removeFromSuperview()
    }
    self.imageNumberLabel.isHidden = true
    self.imagePageControl.isHidden = true
  }
...

}
```

- 참고한 글

[CollectionView cell displaying duplicated data when scrolling?](https://stackoverflow.com/questions/61101597/collectionview-cell-displaying-duplicated-data-when-scrolling)

[[iOS] prepareForReuse란?](https://sueaty.tistory.com/180)

[iOS) prepareForReuse() 사용해서 셀을 초기화해보자](https://gyuios.tistory.com/72)
