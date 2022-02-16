---
title: Swift Lint Rule 정리
date: 2022-02-15 23:10:00
tags: xcode swift ios study lint
---
(나올 때마다 계속해서 추가할 예정)

### Recommand

- Control Statement Violation: `if`, `for`, `guard`, `switch`, `while`, and `catch` statements shouldn't unnecessarily wrap their conditionals or arguments in parentheses. (control_statement)
    - if, for, guard, switch, while, catch 구문에서 조건문을 소괄호로 묶지 않아도 됩니다.

- For Where Violation: `where` clauses are preferred over a single `if` inside a `for`. (for_where)
    - for 문 안에서 if 문으로 조건 확인을 하는 경우에는 for문 내에 where 구문 사용을 권장합니다.

- Type Name Violation: Type name should be between 3 and 40 characters long: 'T' (type_name)
    - type의 이름은 되도록 3~40자 사이의 글자 수를 사용하도록 합니다.

- Cyclomatic Complexity Violation: Function should have complexity 10 or less: currently complexity equals 12 (cyclomatic_complexity)
    - 코드의 복잡도가 12로 너무 복잡합니다.  10이하의 복잡도를 갖도록 권장합니다.
    - 이 오류는 switch 구문에서 11개의 case를 사용했을 때 나타났습니다. 하지만 대체할 수 있는 더 나은 구문이 없어 그대로 사용하였습니다 ㅎㅎ;
    
- Vertical Whitespace Violation: Limit vertical whitespace to a single empty line. Currently 2. (vertical_whitespace)
    - 공백은 1라인만 사용하는 것이 좋습니다. → 2 줄 이상 공백 라인이 연달아 나오면 발생하는 오류
    - 공백을 지우면 해결되는 간단한 오류!
    
- Multiple Closures with Trailing Closure Violation: Trailing closure syntax should not be used when passing more than one closure argument. (multiple_closures_with_trailing_closure)
    - 한 개 이상의 클로저를 포함하는 함수의 경우에는 마지막 클로저의 파라미터를 축약하지 않는 것이 좋습니다.
    
    ```swift
    // 수정 전(completion 지칭을 생략하고 사용)
    UIView.animate(withDuration: 0.25,
                       delay: 0,
                       options: .curveEaseIn,
                       animations: {
          self.dimmedView.alpha = 0.0
          self.view.layoutIfNeeded()
        }) { _ in // 이 부분!
          if self.presentingViewController != nil {
            self.dismiss(animated: false, completion: nil)
          }
        }
      }
    
    // 권장 코드
    UIView.animate(withDuration: 0.25,
                       delay: 0,
                       options: .curveEaseIn,
                       animations: {
          self.dimmedView.alpha = 0.0
          self.view.layoutIfNeeded()
        },
          completion: { _ in // 변경 된 부분!
          if self.presentingViewController != nil {
            self.dismiss(animated: false, completion: nil)
          }
        })
      }
    ```
    

- Function Body Length Violation: Function body should span 40 lines or less excluding comments and whitespace: currently spans 46 lines (function_body_length)
    - 함수의 바디 길이는 40라인 이하를 권장합니다: 현재는 46라인 입니다.
    
- Type Body Length Violation: Type body should span 200 lines or less excluding comments and whitespace: currently spans 237 lines (type_body_length)
    - 타입의 바디 길이는 주석을 제외하고 200라인 이하를 권장합니다: 현재는 237라인입니다.

### Deny

- dentifier Name Violation: Variable name should be between 3 and 40 characters long: 'i'
