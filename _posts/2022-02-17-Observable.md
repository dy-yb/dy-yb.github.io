---
title: MVVM Data Binding
date: 2022-02-17 13:23:00
tags: mvvm study swift observable binding
categories: [ iOS ]
---


토이 프로젝트에 MVVM 패턴을 적용하려고 하는데 MVVM에서 가장 중요한 개념인 data binding을 사용하기 위해서는 Observable에 대한 이해가 필요했다. 

적용하는 과정을 정리했다.

우선 내가 만들고 싶은 기능은

- 사용자가 값을 입력하면 입력 값과 저장되어있는 정보를 통해 결과 값(Double)을 도출하여 결과창(UITextField)에 입력되도록 한다.
- 입력하는 것을 바로바로 인식하여 계산하고, 별도의 button event는 없다.

따라서 바인딩 코드는 다음 처럼 나올 것으로 예상한다

```swift
// ExchangeViewController.swift

viewModel.result.subscribe { value in
	DispatchQueue.main.async {
		self.resultQuantityTextField.text = value
		}
}
```

### ViewModel 작성

“바인딩”은 값이 변화하는 것을 감지하여 UI를 변경하기 때문에, result의 값이 변화하는 것을 관찰해야하므로 result의 자료형은 Observable type으로 지정한다.

```swift
// ExchangeViewModel.swift

class ExchangeViewModel {
	var result: Observable<String> = Observable(resultQuantity)
```

이 때 Observable type은 Swift에서 기본적으로 제공하는 자료형이 아니므로 별도로 직접 정의를 해야한다.

```swift
// Observable.swift

class Observable<T> {
	var value: T
	init(_ value: T) {
		self.value = value
	}
}
```

- value: 변화를 관찰하고자 하는 값을 담을 변수이며 다양한 자료형일 수 있기 때문에 Generic type으로 지정

값이 변화하는 것을 관찰했으니 변화 할 때마다 발생될 특정 '특정 작업'을 `didset` 을 이용하여 적용시킬 것이다. 

```swift
var value: T {
	didSet {
		// '특정 작업' 수행
	}
}
```

그러기 위해서는 당연히 특정 '특정 작업'을 받을 변수도 필요하다. value를 받아서 void를 리턴하는 클로저를 선언하고 해당 변수에 '특정 작업'을 저장했다가 수행 할 수 있도록 한다.

```swift
var listener: ((T) -> Void)?
```

지금 까지 나온 것들을 종합하면,

```swift
// Observable.swift

class Observable<T> {
	var value: T {
		didSet {
			// 값을 받아 '특정 작업' 수행
			self.listener?(value)
		}
	}
	
	private var listner: ((T) -> Void)?

	init(_ value: T) {
		self.value = value
	}
}
```

'특정 작업'를 담을 변수는 선언했으나 '특정 작업'을 정의할 부분을 아직 만들지 못했다. 맨 처음 바인딩하는 부분에서 사용되는 `.subscribe{ }` 를 통해 정의해줄 것이다. 

이 함수가 갖는 역할은 2가지다.

- 특정 '특정 작업'를 실행
- 특정 '특정 작업'를 나중에 didSet에서도 실행할 수 있도록 클로저 변수(`listener`)에 저장

```swift
func subscribe(listener: @escaping (T) -> Void) {
	listener(value) // '특정 작업' 실행
	self.listener = listener // '특정 작업'을 didSet에서 실행하기 위해 저장
}
```

이 함수까지 모두 종합하면 Observable class가 완성된다.

```swift
// Observable.swift

class Observable<T> {
	// value definition
	var value: T {
		didSet {
			// 값을 받아 '특정 작업' 수행
			self.listener?(value)
		}
	}
	
	private var listner: ((T) -> Void)? // '특정 작업'을 저장 할 클로저 변수

	init(_ value: T) {
		self.value = value // value 초기화
	}

	func subscribe(listener: @escaping (T) -> Void) {
		listener(value) // '특정 작업' 실행
		self.listener = listener // '특정 작업'을 didSet에서 실행하기 위해 저장
	}
}
```

### Reference

[[Swift] didSet과 closure를 통한 데이터 바인딩](https://sujinnaljin.medium.com/swift-didset%EA%B3%BC-closure%EB%A5%BC-%ED%86%B5%ED%95%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B0%94%EC%9D%B8%EB%94%A9-5a610d28c220)

[[iOS][DesignPattern] MVVM 과 DataBinding에 대한 간략한 소개](https://velog.io/@kipsong/iOSDesignPattern-MVVM-%EA%B3%BC-DataBinding%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B0%84%EB%9E%B5%ED%95%9C-%EC%86%8C%EA%B0%9C)

[Data Binding in MVVM on iOS](https://fitzafful.medium.com/data-binding-in-mvvm-on-ios-714eb15e3913)

[Data Binding in MVVM on iOS](https://beenii.tistory.com/124)
