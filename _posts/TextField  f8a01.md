---
title: UITextField 글자수 제한하기
date: 2022-02-26 15:00:00 +0900
tags: swift study UITextField
---

글자수를 제한하기 위해서는 우선 UITextFieldDelegate를 채택해야 한다. 
→ editingChange 상태를 감지하여 반응하도록 할 것이기 때문!

채택할 때에는 viewController 클래스에 상속시키거나, extension을 사용하면 된다.

```swift
// 방법 1
class ViewController: UIViewController, UITextFieldDelegate {
	...
}

// 방법 2
extension ViewController: UITextFieldDelegate {
	...
}

// 두가지 방법 모두 뷰 내에서 명시처리도 당연히 필요! 
textField.delegate = self
```

글자수 제한하는 함수를 만들어서 필드 내에 글자가 수정될 때 호출 되도록 할 것이다.

```swift
func checkMaxLength(textField: UITextField, maxLength: Int) {
    if textField.text?.count ?? 0 > maxLength {
      textField.deleteBackward()
   }
}
```

나는 스토리보드를 쓰지않고 ui를 구성하기 때문에 addTarget 함수를 이용해서 함수를 연결시켜준다.

```swift
private lazy var inputUnitsTextField: UITextField = {
    let textField = UITextField()
    textField.addTarget(self, action: #selector(editedTextField(_:)), for: .editingChanged)
    return textField
}()

@objc func editedTextField(_ sender: UITextField) {
	checkMaxLength(textField: sender, maxLength: 5)
}
```

끝!
