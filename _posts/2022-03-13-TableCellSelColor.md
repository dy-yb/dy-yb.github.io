---
title: UITableView Cell 선택시 배경 색 변경 없게 설정하기
date: 2022-03-13 14:32:00 +09:00
tags: study ios 
---

기본적으로 테이블 뷰는 선택된 셀의 배경 색을 회색으로 변경한다. 이 것을 없애보자~~

![화면 기록 2022-03-13 14 37 45 mov](https://user-images.githubusercontent.com/40792935/158046901-38183acb-f1eb-4171-a54e-ef692d26b43d.gif)

코드 한줄로 가능한.. 아주 간단!

```swift
func tableView(_ tableView: UITableView, **cellForRowAt** indexPath: IndexPath) -> UITableViewCell {
    guard let cell = tableView.dequeueReusableCell(withIdentifier: viewController.cellID, for: indexPath) as? cell else {
      return .init()
    }
    **cell.selectionStyle = .none**
    return cell
  }
```

cellForRowAt 이벤트에서 selectionStyle을 none으로 설정해주면 선택 시 배경 컬러가 없어진다.

아래는 사라진 모습!

![화면 기록 2022-03-13 14 38 11 mov](https://user-images.githubusercontent.com/40792935/158046896-8e402a06-168a-46ab-8353-6dfca7eea2b1.gif)

끗!

### References

https://iphonedev.co.kr/iOSDevQnA/118064
