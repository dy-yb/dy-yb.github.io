---
title: swift 무한스크롤 구현하기
date: 2022-09-11 21:30:01 +09:00
tags: iOS swift study UICollectionView scrollView
categories: [ iOS ]
---

인스타그램처럼 무한 스크롤 형태의 뷰를 구성하기 위해서 스크롤 바닥에 닿으면 데이터를 추가로 요청하는 로직의 페이지 네이션을 구현하였다.
![화면 기록 2022-09-11 21 36 45 mov](https://user-images.githubusercontent.com/40792935/189527863-93cc4af1-0b38-43f1-bd74-2a312d5498f2.gif)

```swift
extension ViewController: UICollectionViewDataSource { // UICollectionViewDelegate에 작성해도 됨
	func scrollViewDidScroll(_ scrollView: UIScrollView) { // scrollView가 스크롤 되었을 때 실행 될 이벤트
    let height = scrollView.frame.height // 스크롤뷰의 전체 높이
    let contentSizeHeight = scrollView.contentSize.height // 전체 콘텐츠 영역의 높이
    let offset = scrollView.contentOffset.y // 클릭 위치
    let reachedBottom = (offset > contentSizeHeight - height) // (클릭 지점 + 스크롤뷰 높이 == 전체 컨텐츠 높이) -> Bool

    if reachedBottom { // 스크롤이 바닥에 닿았다면 서버에 추가 데이터 요청
      scrollViewDidReachBottom(scrollView)
    }
  }

  func scrollViewDidReachBottom(_ scrollView: UIScrollView) { // 서버에 데이터 요청 코드
    if let isNext = self.dataList?.isNext {
      if isNext {
        self.currentPage += 1
        self.interactor?.fetchDataList(
          currentPage: self.currentPage,
          pageSize: self.pageSize,
        )
      }
    }
  }
}
```

`scrollViewDidReachBottom` 메소드 내부에서 사용 된 프로퍼티들에 대한 간단한 설명

- `isNext` → 더 요청할 데이터가 남아있는지?
    - 서버로부터 받음
- `currentPage` → 현재 페이지
    - `isNext`가 `true`이면 1 증가한 값을 서버에 요청 시 파라미터로 사용해 다음 페이지를 요청함
- `pageSize` → 1회 요청할 때마다(페이지마다) 받을 데이터의 개수

+) 보다 자연스러운 로딩을 위해서는 타이머와 `UIActivityIndicatorView`를 이용하여 서버 요청시 애니메이션을 삽입하는 방법이 있음!
