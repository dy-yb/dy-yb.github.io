---
title: Capturing Values
date: 2021-05-28 21:33:00
tags: apple_docs swift
---

- [Apple Swift Documentation](https://docs.swift.org/swift-book/LanguageGuide/Closures.html#ID103) 번역본

### Capturing Values

클로저는 정의 된 컨텍스트에서 상수와 변수를 '캡쳐' 할 수 있습니다. 클로저는 상수와 변수가 정의되어있었던 본래의 범위가 더 이상 존재하지 않더라도 바디 내에서 상수들과 변수들의 값을 참조하고 수정할 수 있습니다.

스위프트에서 클로저의 가장 간단한 형식은 다른 함수의 바디 내에 작성하는 중첩함수 입니다. 중첩 함수는 외부의 함수의 어떠한 인자, 상수, 변수든지  캡쳐할 수 있습니다. 

다음 예시는 `makeIncrementer` 라는 함수를 호출합니다. 이 함수는 `incrementer`라는 중첩 함수를 포함합니다. 중첩함수 `Incrementer()`는 `runningTotal`과 `amount`라는 두 변수를 캡쳐합니다. 이 값들을 캡쳐한 후에 `incrementer`는 호출될때마다 `amount` 만큼 `runningTotal`을 증가시키는 클로저로 `makeIncrementer`에 의해 반환됩니다.

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
```

`makeIncrementer`의 반환타입은 `() → Int` 입니다. 이것은 간단한 값이 아닌 함수를 반환한다는 의미입니다. 이 함수는 인자를 가지지 않고, 호출될 때 마다 Int형 변수를 반환합니다. 함수가 어떻게 다른 함수를 반환할 수 있는지 알고 싶다면 다음 문서([반환 타입으로써의 함수타입)](https://docs.swift.org/swift-book/LanguageGuide/Functions.html#ID177)를 보시기 바랍니다.

`makeIncrementer(forIncrement:)` 함수는 `runningTotal` 함수를 호출할 때 현재 `incrementer`의 `running total`값을 저장하는 정수형 변수를 정의하며 이 변수는 0으로 초기화 됩니다.  

`makeIncrementer(forIncrement:)` 함수는 argument label은 `forIncrement`이고 parameter name은 `amount`인 하나의 정수형 파라미터를 가지고 있습니다. 

- Argument label: 외부에서 함수를 호출 할 때 사용되는 파라미터의 이름, '_'를 사용하여 생략 가능
- Parameter name: 함수 내부에서 변수로서 사용될 때 불리는 이름

```swift
func functionName(argumentLabel parameterName: valueType) -> returnType{
    parameterName = 0
}
functionName(argumentLabel: 100)
```

인자 값은 runningTotal이 incrementer 함수가 호출되어 반환될 때 마다 얼만큼 증가 될 것인지 파라미터를 통해 지정하고 전달합니다. makeIncrementer 함수는 실제로 값을 증가시키는 기능을 하는 incementer 중첩 함수를 정의 합니다. 이 기능은 단순히 amount를 runningTotal에 값을 더하고 결과를 반환하는 것입니다.

중첩 함수 incrementer만 별도로 보면 조금 이상하게 느껴질 수 있습니다:

```swift
func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
}
```

incrementer() 함수는 아무런 파라미터도 없는데 함수 바디 내에서 runningTotal과 amount를 참조하고 있습니다. 이것은 캡쳐링을 통해 둘러싸인 외부 함수로부터 runningTotal과 amount를 참조하여 내부의 본인이 가진 함수 바디에서 사용합니다. 참조를 캡쳐하는 것은 makeIncrementer 함수 호출이 종료될 때 runningTotal과 amount가 사라지지 않도록 하고, 다음에 incrementer 함수가 다시 호출 될 때 runningTotal을 사용할 수 있도록 합니다.

> 최적화를 위해, Swift는 값을 복사하여 저장하고 캡쳐하는 것 대신에 (나중에)

아래는 실행 시 `makeIncrementer`의 예시입니다.

```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
```

위 코드는 호출될 때마다 `runningTotal` 변수에 10을 더하는 증가 함수를 참조하도록 `incrementByTen` 상수를 설정하고 있습니다. 함수가 여러번 호출되면 실행시 다음과 같은 결과가 나옵니다.

```swift
incrementByTen()
// returns a value of 10
incrementByTen()
// returns a value of 20
incrementByTen()
// returns a value of 30
```

만약 두번째 증가자를 생성하면 그 증가자만의 새로운 별도의 runningTotal 변수를 갖게됩니다.

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven()
// returns a value of 7
```

원래의 증가자를 다시 호출하면(`incrementByTen`) 본인의 `runningTotal`변수가 이어서 증가되고, `incrementBySeven`으로 캡처된 변수는 영향을 주지 않습니다.

```swift
incrementByTen()
// returns a value of 40
```

NOTE: 클래스 인스턴스 요소에 클로저를 할당하고, 클로저가 인스턴스 혹은 멤버를 참조하여 그 인스턴스를 캡처하면, 클로저와 인스턴스 사이에 강한 참조 사이클이 형성됩니다. 스위프트에서는 캡처리스트를 통해 그들의 강한 참조 사이클을 깨트립니다. 더 자세한 내용을 원한다면 다음 글을 참조하십시오. Strong Reference Cycles for Closures.
