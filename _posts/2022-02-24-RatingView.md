---
title: 별점 입력 뷰 구현
date: 2022-02-24 14:40:00 +0900
tags: swift study UIStackView UISlider
categories: [ iOS ]
---

UIStackView와 UISlider를 별점을 입력하는 뷰를 구현하였다.

- 요구 사항 상세
    - 별점은 1 ~ 5점 까지 가능(0점은 불가능 하고, 0.5 단위는 없음)
    - 슬라이드 방식으로도 움직일 수 있고, 원하는 위치를 클릭하여 바로 넘어가기도 가능

결과를 미리 보면 요렇다!

![Slider mov](https://user-images.githubusercontent.com/40792935/155464373-49f648fc-62bc-4792-ade4-7845da3c4d49.gif)


우선은 사용할 별 이미지를 준비하고, 스택 뷰에 별 이미지를 담은 이미지 뷰 5개를 나열해 줄 것이다.

- 이미지는 iconfinder에서 빈 별, 채워진 별 두가지 버전이 있는 것으로 다운 받았음
    - [Iconfinder](https://www.iconfinder.com/)
        - 회원가입시 색상을 원하는 대로 바꿀 수 도 있음
        - 저작권 문제가 있을 수 있으니 free 버전으로 필터를 걸어두고 검색하면 편함!
        
- 코드

```swift
// UI
lazy var ratingStarStackView: UIStackView = {
    let stackView = UIStackView()
    stackView.translatesAutoresizingMaskIntoConstraints = false
    stackView.axis = .horizontal
    stackView.alignment = .center
    stackView.distribution = .equalSpacing
    stackView.spacing = 10
    return stackView
  }()
private var starImageViews: [UIImageView] = []

// 반복문을 이용하여 Stack View에 별 Image View 추가
func setRatingImageView() {
    for index in 0..<5 {
      let imageView = UIImageView()
      imageView.image = UIImage(named: "ic_rating_off")
      imageView.tag = index
      ratingStarStackView.addArrangedSubview(imageView)
      starImageViews.append(ratingStarStackView.subviews[index] as? UIImageView ?? UIImageView())
    }
  }
```

스택뷰에서 가장 중요한 것은 정렬과 배열 설정인 것 같다. 나에게 맞는 설정을 찾기위해서 정말 적용 안해본 조합이 없다.... 다음번에 스택뷰에 대해서 자세히 다시 정리하는 글을 적어보려고 한다.

간단하게 스택뷰에 적용한 옵션들만 간단히 설명하고 넘어가 보자면,

- stackView.**axis** = .horizontal
    - Axis는 축을 설정합니다.
    - 쉽게 말해 왼쪽에서 오른쪽으로 나열할건지(horizontal), 위에서 아래로 쌓아나갈 것인지(vertical)를 결정합니다.
- stackView.**alignment** = .center
    - Alignment는 정렬입니다.
    - fill, leading, top, firstBaseline, center, trailing, bottom, lastBaseline 옵션이 있습니다.
    - center는 가운데 정렬! (너무 많아서 나머지는 생략)
- stackView.**distribution** = .equalSpacing
    - Distribution은 어떻게 공간을 분배하여 뷰를 배치할 것인지 설정합니다.
    - fill, fillEqually, fillProportionally, equalSpacing, equalCentering 옵션이 있습니다.
    - equalSpacing은 StackView가 포함하는 view들이 사용하고 남은 공간을 사이좋게 똑같이 나누어 갖도록 배치합니다.
- stackView.**spacing** = 10
    - spacing은 최소한의 여백을 설정해줍니다.
    - distribution을 통해 자동으로 여백이 정해지는데 아무리 스택뷰가 좁더라도 최소한 이 만큼은 떨어지도록 할 수 있습니다.
    - 만약에 여백때문에 뷰가 존재할 공간이 부족하다면 뷰의 사이즈를 줄여버립니다.

아무튼 어찌저찌 스택 뷰에 별들이 옹기종기 잘 모였다면, 이제 Slider를 넣어줄 차례가 되겠다.

- 코드

```swift
// UI
// TapUISlider는 하단에서 만들겁니다. 탭 방식이 필요없다면 그냥 UISlider를 상속하면 됩니다.
lazy var ratingSlider: TapUISlider = {
    let slider = TapUISlider()
    slider.translatesAutoresizingMaskIntoConstraints = false
    slider.maximumValue = 5 // 최대값
    slider.minimumValue = 0 // 최소값
		// 실제로 slider는 사용자 눈에 보이지 않을 것이므로 모든 컬러는 clear로 설정
    slider.maximumTrackTintColor = .clear 
    slider.minimumTrackTintColor = .clear
    slider.thumbTintColor = .clear
    slider.addTarget(self, action: #selector(tapSlider(_:)), for: .valueChanged)
    return slider
}()

// 이 함수가 아주아주 중요하다!
@objc func tapSlider(_ sender: UISlider) {
		// 슬라이더의 값을 올림 하여 받아옴 (사이에서 멈출 때는 더 큰 수에 반응)
    var intValue = Int(ceil(sender.value))
    
    for index in 0..<5 {
      if intValue == 1 {
        intValue -= 1 // index에 맞추기 위해 -1
        starImageViews[index].image = UIImage(named: "ic_rating_on")
      } else if index < intValue { // intValue 보다 작은 건 칠하기
        starImageViews[index].image = UIImage(named: "ic_rating_on")
      } else { // intValue 보다 크면 빈 별 처리
        starImageViews[index].image = UIImage(named: "ic_rating_off")
      }
   }
}

// Slider와 Stackview의 위치를 동일하게 하여 덮어버리기
func layout() {
    NSLayoutConstraint.activate([
			ratingSlider.topAnchor.constraint(equalTo: ratingLabel.bottomAnchor, constant: 10),
      ratingSlider.centerXAnchor.constraint(equalTo: view.safeAreaLayoutGuide.centerXAnchor),
      ratingSlider.widthAnchor.constraint(equalTo: view.safeAreaLayoutGuide.widthAnchor, constant: -200),
      ratingSlider.heightAnchor.constraint(equalToConstant: 50),
      
      ratingStarStackView.topAnchor.constraint(equalTo: ratingSlider.topAnchor),
      ratingStarStackView.centerXAnchor.constraint(equalTo: view.safeAreaLayoutGuide.centerXAnchor),
      ratingStarStackView.widthAnchor.constraint(equalTo: ratingSlider.widthAnchor),
      ratingStarStackView.heightAnchor.constraint(equalTo: ratingSlider.heightAnchor),
		])
 }
```

요기까지 하면 슬라이딩 방식으로 별점을 줄 수 있는 형태가 된다. 근데 이제 탭 방식으로도 바로바로 입력 하면 편하니까 탭도 되는 슬라이더를 만들어 상속 시켜 주면~~

```swift
class TapUISlider: UISlider {
  override func beginTracking(_ touch: UITouch, with event: UIEvent?) -> Bool {
    let width = self.frame.size.width
    let tapPoint = touch.location(in: self)
    let fPercent = tapPoint.x/width
    let nNewValue = self.maximumValue * Float(fPercent)
    if nNewValue != self.value {
      self.value = nNewValue
    }
    return true
  }
}
```

끝!

### References
[swift Slider 값 변경, 별점 기능 구현하기](https://42kchoi.tistory.com/271)

[별점 슬라이더 만들기](https://iamcho2.github.io/2021/06/22/make-star-rating-view)

[](https://hyunndyblog.tistory.com/148)
