---
title: 실수형 변수 소수점 처리하기
date: 2022-02-21 13:10:00
tags: swift study truncatingRemainder
---

일단 나는 Double형 변수를 String으로 변환하여 UITextField에 표시하는 것이 목적이었다.

소수점을 전부 표현하는 것은 불필요하기 때문에 소수점 처리하는 함수에 대해서 알아봤다.


Swift에서는 반올림, 올림, 버림, 이렇게 세 가지 함수를 지원하고 있었다.

### 반올림(Round)

- 소수점 이하의 수를 반올림 처리하는 함수

```swift
let result: Double = 3.14524
let result2: Double = 3.5123

print(round(result)) // 3
print(round(result2)) // 4
```

### 올림(Ceil)

- 소수점 이하의 수를 전부 버리고 정수부에 1을 더함

```swift
let result: Double = 3.14524
let result2: Double = 3.5123

print(round(result)) // 4
print(round(result2)) // 4
```

### 버림(floor)

- 소수점 이하의 수를 전부 버림 처리

```swift
let result: Double = 3.14524
let result2: Double = 3.5123

print(round(result)) // 3
print(round(result2)) // 3
```

이 함수들의 약간의 문제라고 한다면(특히 나에게는..) 소수점 이하의 숫자를 몽땅 처리해버리기 때문에 적당히 둘째짜리 수까지는 남기고 싶을 경우에 다른 조치가 필요했다.. 찾아보니 약간의 꼼수같은 방법이 있었다.

남기고 싶은 자리수만큼 10을 곱하고 반올림, 버림, 올림 처리를 한 후 다시 곱한 수만큼 나누는 방법이다.

```swift
// 셋 째 자리까지 반올림하기
let result: Double = 3.14524

result = round(result * 100) / 100
// result*100 = 314.524
// round(result * 100) = 315
// round(result * 100) / 100 = 3.15
```

이렇게 하면 셋째 자리 소수에서 반올림하고 소수점 둘째짜리 까지는 살려낼 수 있다!

이 형태의 숫자를 UILabel이나 UITextField에 나타나기 위해서는 String 형태로 변환해야하는데,

```swift
resultTextField.text = String(format: "%.2f", result)
```

요렇게 작성하면 둘째자리 수까지 잘 나타나는 것을 확인할 수 있다.

그러나 여기서 마무리하면 좋겠지만.. 또 다른 문제로 소수점이 없는 숫자도 계속 “.0”이 붙어서 나타났다....

<img width="216" alt="스크린샷 2022-02-22 13 00 43" src="https://user-images.githubusercontent.com/40792935/155061884-c1fb80ca-0d71-4d69-b7e6-dbdfc30ed465.png">

<img width="216" alt="스크린샷 2022-02-22 12 59 55" src="https://user-images.githubusercontent.com/40792935/155061917-92095ea6-8a7f-462d-8136-ca2612f5493e.png">

요딴식으로........ 가독성만 떨어지게 만드는 0파티..ㅠㅠ

.00 은 아무짝에 쓸모 없고 지저분하기만 하기때문에, 정수값이 나올 경우에는 “0”, “200” 이렇게 깔끔하게 보이게 하고싶었다. 

그래서 찾아봤지만 Swift에서는 isInteger와 같은 함수는 없었고.... 머리를 굴려봤고..

모드 수식을 이용해 1로 나누었을 때 나머지가 있으면 소수점을 가진 숫자이므로 조건문을 걸면 어떨까 싶어 바로 적용해봤다.

```swift
if result % 1 != 0 {
      resultTextField.text = String(format: "%.2f", resultQuantity)
    } else {
      resultTextField.text = String(Int(resultQuantity))
    }
```

그러나 역시나 호락호락하지 않았고..

- '%' is unavailable: For floating point numbers use truncatingRemainder instead

![스크린샷 2022-02-22 13 03 33](https://user-images.githubusercontent.com/40792935/155061936-b25eb972-fad9-415b-a86f-5aa71d6aff13.png)

정수가 아니면 모드(%)를 쓸 수 없단다.. 그래도 실수형 숫자는 truncatingRemainder를 쓰라고 알려주는 꽤나 친절한 에러였다.

- 진짜_찐으로_최종_찐_최종.code

```swift
let result: Double = 3.14524

result = round(result * 100) / 100

if result.truncatingRemainder(dividingBy: 1) != 0 {
      resultTextField.text = String(format: "%.2f", result)
    } else {
      resultTextField.text = String(Int(result))
    }
```

![스크린샷 2022-02-22 13 14 25](https://user-images.githubusercontent.com/40792935/155061997-7567f02b-02dd-48a6-9439-bb6cd24e1e47.png)

끝!
