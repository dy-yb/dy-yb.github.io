---
title: UILabel text에 더보기 붙이기
date: 2022-06-26 22:10:00 +09:00
tags: iOS swift study label
categories: [ iOS ]
---

긴 글의 내용이 일정한 길이로 축약되어 나타나는 label 만들기

- 원하는 모양
    - 최대 두 줄까지 내용이 보이고 그 뒤에 “… 더보기"가 붙는 형태

<img width="311" alt="스크린샷_2022-06-26_17 57 51" src="https://user-images.githubusercontent.com/40792935/175811373-980f6f2f-19e8-44f5-88ad-807bdcb7f211.png">


```swift
//
//  UILabel+Ext.swift

import UIKit

extension UILabel {

// label의 폰트, 사이즈를 계산해서 최종적으로 화면에 보여질 글자의 길이
  var visibleTextLength: Int { 
    let font: UIFont = self.font 
    let mode: NSLineBreakMode = self.lineBreakMode 
    let labelWidth: CGFloat = self.frame.size.width 
    let labelHeight: CGFloat = self.frame.size.height
    let sizeConstraint = CGSize(width: labelWidth, height: CGFloat.greatestFiniteMagnitude) // Label의 크기

    if let myText = self.text {

      let attributes: [AnyHashable: Any] = [NSAttributedString.Key.font: font]
      let attributedText = NSAttributedString(
        string: myText,
        attributes: attributes as? [NSAttributedString.Key: Any])

      let boundingRect: CGRect = attributedText.boundingRect(
        with: sizeConstraint,
        options: .usesLineFragmentOrigin,
        context: nil)

      if boundingRect.size.height > labelHeight {
        var index: Int = 0
        var prev: Int = 0
        let characterSet = CharacterSet.whitespacesAndNewlines
        repeat {
          prev = index
          if mode == NSLineBreakMode.byCharWrapping {
            index += 1
          } else {
            index = (myText as NSString).rangeOfCharacter(
              from: characterSet,
              options: [],
              range: NSRange(
                location: index + 1,
                length: myText.count - index - 1)).location
          }
        } while index != NSNotFound && index < myText.count && (myText as NSString)
          .substring(to: index)
          .boundingRect(
            with: sizeConstraint,
            options: .usesLineFragmentOrigin,
            attributes: attributes as? [NSAttributedString.Key: Any], context: nil)
          .size
          .height <= labelHeight

        return prev
      }
    }

    if self.text == nil {
      return 0
    } else {
      return self.text!.count
    }
  }

// Label에 "... 더보기"와 같은 텍스트를 추가하기 위한 함수
  func addTrailing(
    with trailingText: String,
    moreText: String,
    moreTextFont: UIFont,
    moreTextColor: UIColor
  ) {

    let readMoreText: String = trailingText + moreText

    if self.visibleTextLength == 0 { return }

    let lengthForVisibleString: Int = self.visibleTextLength

    if let myText = self.text {

      let mutableString: String = myText
      let trimmedString: String? = (mutableString as NSString).replacingCharacters(
        in: NSRange(
          location: lengthForVisibleString,
          length: myText.count - lengthForVisibleString
        ), with: "")

      let readMoreLength: Int = (readMoreText.count)

      guard let safeTrimmedString = trimmedString else { return }

      if safeTrimmedString.count <= readMoreLength { return }

      let trimmedForReadMore: String = (safeTrimmedString as NSString)
        .replacingCharacters(
          in: NSRange(
            location: safeTrimmedString.count - readMoreLength,
            length: readMoreLength)
          ,with: ""
        ) + trailingText

      let answerAttributed = NSMutableAttributedString(
        string: trimmedForReadMore,
        attributes: [NSAttributedString.Key.font: self.font as Any]
      )

      let readMoreAttributed = NSMutableAttributedString(
        string: moreText,
        attributes: [NSAttributedString.Key.font: moreTextFont,
                     NSAttributedString.Key.foregroundColor: moreTextColor]
      )
      answerAttributed.append(readMoreAttributed)
      self.attributedText = answerAttributed
    }
  }
}
```

- 사용방법
    - 이 때 Label의 라인 수는 0으로 설정해두고, 실제로 나타날 라인 수는 Label의 높이로 맞추면 된다.
    - font와 line height을 계산해서 자동으로 설정하면 좋겠지만, 일단 50으로 고정하여 사용했더니 2줄이 나타남!

```swift
let contentLabel = UILabel().then {
let contentLabel = UILabel().then {
    $0.translatesAutoresizingMaskIntoConstraints = false
    $0.numberOfLines = 0 
    $0.font = UIFont(name: "Apple SD Gothic Neo", size: 13)
}

guard let contentTextLength = self.contentLabel.text?.count else { return }

if contentTextLength > 1 {
   DispatchQueue.main.async {
     self.contentLabel.addTrailing(with: "... ", moreText: "더보기", moreTextFont: .systemFont(ofSize: 13), moreTextColor: UIColor.gray)
   }
}
```

- 참고한 글

[Add "...Read More" to the end of UILabel](https://stackoverflow.com/questions/32309247/add-read-more-to-the-end-of-uilabel)
