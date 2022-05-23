---
title: tint color와 text color
date: 2022-04-04 23:30:00 +09:00
tags: study ios uiview tintcolor
categories: [ iOS ]
---

버튼의 글씨 색상을 변경하는 방법들을 찾아보던 중에 tintColor, textColor 두 속성의 차이점이 궁금해져 찾아보게 되었다.. 두 속성 모두 UIButton에만 국한되지 않고 다른 ui에서도 자주 만날 수 있었기 때문에..!

- TintColor
    - 시각적으로 현재 화면 상에서 어떤 컴포넌트가 활성화되었는지를 보여주는 요소
    - 기본적으로 TintColor는 `UIView` 의 프로퍼티로 존재하기 때문에 `UIView`
    를 상속받는 뷰들은 모두 TintColor를 적용 가능
- text color
    - 단순히 컴포넌트 내부의 라벨의 텍스트 컬러

### Reference
[[ios] 이미지에 tintColor 적용하기](https://baked-corn.tistory.com/66)

[What is difference between textColor and tintColor in iOS?](https://stackoverflow.com/questions/21931876/what-is-difference-between-textcolor-and-tintcolor-in-ios)
