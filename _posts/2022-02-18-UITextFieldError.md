---
title: Can not find keyplane that supports type 4 for keyboard error
date: 2022-02-18 18:30:00
tags: study swift simulator iOS UITextField
---

UITextField keyboard type을 numberpad로 변경하고 빌드했더니 다음 에러가 떴다.

```swift
Can't find keyplane that supports type 4 for keyboard iPhone-PortraitTruffle-
NumberPad; using 27071_PortraitTruffle_iPhone-Simple-Pad_Default
```

이 에러가 키패드가 나타나지 않는 원인은 아닌 것 같고 해결방법은 아래와 같다고 한다. (에러는 무시해도 무관하다)

### 해결 방법

아주 간단~~

<aside>
🔎 iOS Simulator → I/O → Keyboard → Connect Hardware Keyboard(Uncheck로 변경)

</aside>

re-build 하지 않아도 아주 잘 키보드가 나오는 것을 확인 할 수 있었다!

[참고](https://developer.apple.com/forums/thread/126616)
