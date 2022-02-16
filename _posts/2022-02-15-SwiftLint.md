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
    

### Deny

- dentifier Name Violation: Variable name should be between 3 and 40 characters long: 'i'
