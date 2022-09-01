---
title: UISearchController SearchBar 취소 버튼 커스텀하기
date: 2022-09-01 12:30:01 +09:00
tags: iOS swift study searchbar searchController
categories: [ iOS ]
---
UISearchController를 사용하면 Search Bar를 클릭했을 때 Cancel 버튼이 활성화 된다. 버튼의 색상과 내용을 변경해보자~!

- Default ui

![화면 기록 2022-09-01 12 20 13 mov](https://user-images.githubusercontent.com/40792935/187826499-e91984b2-0c09-4781-8512-507e02dfddcf.gif)

취소 버튼의 title과, 색상을 변경하는 코드

```swift
let searchController = UISearchController()

// Change placeholder
searchController.searchBar.placeholder = "검색어를 입력하세요."
// Change text color
searchController.searchBar.tintColor = .black 
// Change Cancel button value 
searchController.searchBar.setValue("취소", forKey: "cancelButtonText") 
```

- 적용 결과

![화면 기록 2022-09-01 12 22 10 mov](https://user-images.githubusercontent.com/40792935/187826487-03529161-3b6f-4f0f-b9c4-824c3cd590e9.gif)
