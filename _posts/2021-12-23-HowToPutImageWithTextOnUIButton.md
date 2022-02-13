---
title: UIButton에 텍스트랑 이미지 둘 다 넣기
date: 2021-12-23 14:20:00
tags: xcode swift ios study cocoapods
---

```swift
let sortButton: UIButton = {
      let button = UIButton(frame: CGRect(x: frame.size.width - 74, y: 10, width: 54, height: 30))
      button.addTarget(self, action: #selector(ChallengeViewController.buttonPressed(_:)), for: .touchUpInside)
      button.setTitle("최신순", for: .normal)
      button.setImage(#imageLiteral(resourceName: "ic_table_sort"), for: .normal)
      button.titleLabel?.font = .systemFont(ofSize: 13)
      button.setTitleColor(.gray50, for: .normal)
      button.contentHorizontalAlignment = .center
      button.semanticContentAttribute = .forceRightToLeft //중요
      return button
    }()
```
