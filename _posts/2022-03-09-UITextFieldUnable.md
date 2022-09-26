---
title: TextField 입력제한
date: 2022-03-09 22:00:00 +0900
tags: swift study Firebase Database Storage
categories: [ iOS ]
---

UITextField 입력 불가 상태로 변환하고, 커서색상을 투명하게 전환하는 코드

```swift
private lazy var resultUnitsTextField: UITextField = {
    let textField = UITextField()
    textField.tintColor = .clear // 커서 색상 투명으로 변경
    return textField
  }()

extension ExchangerViewController: UITextFieldDelegate {
  func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
    return false // 입력 불가
  }
}
```
