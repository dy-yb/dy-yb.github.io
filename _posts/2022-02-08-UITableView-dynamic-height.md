---
title: TableView의 높이를 동적으로 조절하기
date: 2022-02-08 09:10:00
tags: xcode swift ios study UITableView
---

- TableView의 rowHeight 속성에 AutometicDimension을 설정하여 Table 세로 길이가 유동적으로 변한 다는 것을 선언해줘야 함

```swift
 func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return tableView.rowHeight
    }
```

```swift
   override func viewDidLoad(){
     super.viewDidLoad();
      myTableView.rowHeight = UITableView.automaticDimension
   }
```

- 레퍼런스: [https://velog.io/@tnddls2ek/TableView-동적인-셀-높이-조절](https://velog.io/@tnddls2ek/TableView-%EB%8F%99%EC%A0%81%EC%9D%B8-%EC%85%80-%EB%86%92%EC%9D%B4-%EC%A1%B0%EC%A0%88)
