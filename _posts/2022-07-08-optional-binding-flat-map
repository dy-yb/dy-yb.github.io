---
title: 보다 간결한 옵셔널 바인딩(flatmap, map)
date: 2022-07-08 00:00:01 +09:00
tags: iOS swift study optional flatmap map
categories: [ iOS ]
---

간혹 옵셔널 바인딩이 연속되어 가독성을 떨어트리는 경우가 있습니다. 

예를 들어 다음과 같은 코드가 있을 때,

```swift
if let image = participants[index].image {
   if let url = URL(string: image) {
      if let data = try? Data(contentsOf: url) {
          imageView.image = UIImage(data: data)
			}
	 }
} else {
   imageView.image = UIImage(name: "default_image")
}
```

매번 if let으로 타고 들어가지 않고 `,` 를 이용하여 묶어버리는 방법도 가능하지만,

```swift
if let image = images[index],
   let url = URL(string: image),
   let data = try? Data(contentsOf: url) {
	 imageView.image = UIImage(data: data)
} else {
   imageView.image = UIImage(name: "default_image")
}
```

더 간결하게 표현하고 싶다면 고차함수 `flatmap` 을 이용할 수 있습니다.

```swift
$0.image = images[index]
   .flatMap { URL(string: $0) } // $0 = image
   .flatMap { try? Data(contentsOf: $0) } // $0 = url
   .flatMap { UIImage(data: $0) } // $0 = data
   ?? KKDS.Image.ic_person_24 // else에 해당하는 부분, 옵셔널 체이닝 할 때와 동일
```

추가로 단일 옵셔널 바인딩은 `map` 을 이용하여 간결화 할 수 있습니다.

```swift
if let number = number {
	print("\(number)")
} else {
	print("0")
}

// map 적용
number.map { print("\($0)") } ?? print("0")
```

- 참고

[[Swift] 간결한 if let 문은 Optional의 map 그리고 Nil-Coalescing(??)으로 대체하기](http://minsone.github.io/programming/swift-optional-map-nil-coalescing)
