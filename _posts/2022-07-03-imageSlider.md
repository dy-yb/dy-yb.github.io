---
title: Pagecontrol과 Scrollview를 이용하여 이미지 슬라이드 만들기
date: 2022-07-03 20:10:00 +09:00
tags: iOS swift study pagecontrol
categories: [ iOS ]
---

ScrollView와 PageControl을 이용해서 이미지 슬라이드를 만들어 보았습니다.

- 실행 화면

![화면_기록_2022-06-29_15 03 55 mov](https://user-images.githubusercontent.com/40792935/177043092-f6287609-a110-47ca-b489-cf5f7da7d0d7.gif)

- 전체 코드

```swift
lazy var imageScrollView = UIScrollView().then {
    $0.translatesAutoresizingMaskIntoConstraints = false
    $0.isPagingEnabled = true
    $0.showsHorizontalScrollIndicator = false
    $0.delegate = self
  }

  private let imagePageControl = UIPageControl().then {
    $0.translatesAutoresizingMaskIntoConstraints = false
    $0.currentPage = 0
    $0.pageIndicatorTintColor = UIColor(red: 255/255, green: 255/255, blue: 255/255, alpha: 0.5)
    $0.currentPageIndicatorTintColor = .white
    $0.hidesForSinglePage = true
  }

private let imageNumberLabel = UILabel().then {
    $0.translatesAutoresizingMaskIntoConstraints = false
    $0.backgroundColor = UIColor(red: 34/255, green: 34/255, blue: 34/255, alpha: 0.5)
    $0.layer.cornerRadius = 14
    $0.clipsToBounds = true
    $0.text = "0/1"
    $0.font = .systemFont(ofSize: 12, weight: .semibold)
    $0.textColor = .white
  }

func setImageSlider(images: [String]) { // scrolliVew에 imageView 추가하는 함수
    for index in 0..<images.count {

      let imageView = UIImageView()
      imageView.image = UIImage(named: images[index])
      imageView.contentMode = .scaleAspectFit
      imageView.layer.cornerRadius = 5
      imageView.clipsToBounds = true

      let xPosition = self.contentView.frame.width * CGFloat(index)

      imageView.frame = CGRect(x: xPosition,
                               y: 0,
                               width: self.contentView.frame.width,
                               height: self.contentView.frame.width)

      imageScrollView.contentSize.width = self.contentView.frame.width * CGFloat(index+1)
      imageScrollView.addSubview(imageView)
    }
  }

extension FeedListCell: UIScrollViewDelegate { 
  func scrollViewDidScroll(_ scrollView: UIScrollView) { // scrollView가 스와이프 될 때 발생 될 이벤트
    self.imagePageControl.currentPage = Int(round(imageScrollView.contentOffset.x / UIScreen.main.bounds.width))
    self.imageNumberLabel.text = "\(imagePageControl.currentPage)/\(imagePageControl.numberOfPages)"
  }
}
```

- 참고한 글

[Making Image Slider with UIScrollView in Swift](https://stackoverflow.com/questions/38835534/making-image-slider-with-uiscrollview-in-swift)

[[ios] Horizontal Scroll View](https://baked-corn.tistory.com/35)

[Programmatically Linking UIPageControl to UIScrollView](https://stackoverflow.com/questions/10198732/programmatically-linking-uipagecontrol-to-uiscrollview)

[Aug 25, 2021, TIL (Today I Learned) - 이미지 전환과 UIPageControl 그리고 데이터 전달](https://velog.io/@inwoodev/Aug-25-2021-TIL-Today-I-Learned-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%A0%84%ED%99%98%EA%B3%BC-UIPageControl)
