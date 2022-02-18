---
title: Can't find keyplane that supports type 4 for keyboard error
date: 2022-02-18 12:00:00
tags: study swift simulator iOS UITextField
---

UITextField keyboard type을 numberpad로 변경하고 빌드했더니 다음 에러가 떴다.

```swift
Can't find keyplane that supports type 4 for keyboard iPhone-PortraitTruffle-
NumberPad; using 27071_PortraitTruffle_iPhone-Simple-Pad_Default
```

이유는 내가 사용하고있는 시뮬레이터에 아직 해당 키보드가 연결되어있지 않아서 나타나는 듯 하다.

### 해결 방법

아주 간단~~

<aside>
🔎 iOS Simulator → I/O → Keyboard → Connect Hardware Keyboard(Uncheck로 변경)

</aside>

re-build 하지 않아도 아주 잘 키보드가 나오는 것을 확인 할 수 있었다!

[참고](https://developer.apple.com/forums/thread/126616)
