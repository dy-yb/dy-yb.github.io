---
title: UITableViewCell 의 간격 조정과 둥근 테두리 효과 적용
date: 2022-02-23 13:10:00
tags: swift study UITabelView UITableViewCell
categories: [ iOS ]
---

- 코드

```swift
class TableViewCell: UITableViewCell {
    
  override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
    super.init(style: style, reuseIdentifier: reuseIdentifier)
    setView()
  }
  
  override func layoutSubviews() {
    super.layoutSubviews()
		// Cell 간격 조정
    contentView.frame = contentView.frame.inset(by: UIEdgeInsets(top: 6, left: 6, bottom: 6, right: 6))
  }
  
  required init?(coder: NSCoder) {
    super.init(coder: coder)
  }
  
  func setView() {
		// Cell 둥근 모서리 적용(값이 커질수록 완만)
    contentView.layer.cornerRadius = 10
  }
}
```

- 결과

<img width="452" alt="스크린샷_2021-12-13_오후_11 28 25" src="https://user-images.githubusercontent.com/40792935/155285026-a410e680-99f8-473c-930c-c18d211620b4.png">



### Reference

[[iOS - swift] tableView cell 간 간격 설정, cell 선택 UI (contentView.frame.inset, setSelected)](https://ios-development.tistory.com/655)
