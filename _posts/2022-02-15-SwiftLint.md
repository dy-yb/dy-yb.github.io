---
title: Swift Lint Rule 정리
date: 2022-02-15 23:10:00
tags: xcode swift ios study lint
categories: [ iOS ]
---

Lint라는 것.. 사용해보니 굉장히 예민하고도 예민한 놈이었다.
하지만 나는 아직은 Swift에서 추구하는 코드 스타일이 어떤 건지 익히는 단계에 있다고 생각해서 무작정 해제 시키는 것을 지양하고
그들이 불편~하게 느끼는 걸 찾아보고 정리해 두었다가 차차 방해가 되는 것을 지워나가면 좋겠다고 생각해 기록하기로 했다!

(나올 때마다 계속해서 추가할 예정 - Updated on 2022.03.23)

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
  

- Nesting Violation: Types should be nested at most 1 level deep (nesting)
    - (번역을 통해 추측하였음)
    - nested class 라는 뜻이 클래스 내부에 존재하는 클래스 라는 것을 지칭 한다는 것으로 보아 타입을 정의할때는 어떤 것에 속하지 않고 가장 윗 단에 있어야 한다는 뜻인 것 같았고,
    - 실제로도 최상위 레벨에 정의를 하니 lint가 만족했는지 조용해졌다^^.  
    
    ```swift
    // 예를 들어서 이 상태에서는 린트 레이더망에 걸리게 됨
    class TestClass {
    	...
    	enum Number { // Nest violation!!
    		case one
    		case two
    	}
    	...
    }
    
    // 수정이랄 것도 없고 분리 시켜버리면 된다~!
    enum Number { // Nest violation!!
    	case one
    	case two
    }
    
    class TestClass {
    	...
    }
    ```
    
- Self in Property Initialization Violation: `self` refers to the unapplied `NSObject.self()` method, which is likely not expected. Make the variable `lazy` to be able to refer to the current instance or use `ClassName.self`. (self_in_property_initialization)
    - self를 사용할 때는 lazy 변수를 사용하거나 `ClassName.self` 를 사용하십시오.
    
    ```swift
    // lazy가 아닌 let, var 변수에서는 에러!
    class View: UIView {
        let button: UIButton = {
            let button = UIButton()
            button.addTarget(self, action: #selector(didTapButton), for: .touchUpInside) // 여기~~
            return button
        }()
    }
    
    //lazy를 붙여줌
    class View: UIView {
        lazy var button: UIButton = {
            let button = UIButton()
            button.addTarget(self, action: #selector(didTapButton), for: .touchUpInside)
            return button
        }()
    }
    ```
    
    - lazy란?
        - 지연 저장 프로퍼티로 인스턴스가 초기화 되는 시점이 아니라 속성에 처음 접근하는 시점에 초기화 하도록 함
        
- Unused Closure Parameter Violation: Unused parameter "error" in a closure should be replaced with _. (unused_closure_parameter)
    - 사용하지 않는 클로저 파라미터의 경우 `_`으로 바꿔서 작성하는게 좋습니다.

- No Space in Method Call Violation: Don't add a space between the method name and the parentheses. (no_space_in_method_call)
    - 메소드 이름과 괄호 `()` 사이에는 공백이 있으면 안됩니다.
    
    ```swift
    configure () // X
    configure() // O
    ```


### Deny

- dentifier Name Violation: Variable name should be between 3 and 40 characters long: 'i'
- Type Name Violation: Type name should start with an uppercase character: 'exchangerTextFieldTag' (type_name)
    - Type name의 첫 글자는 반드시 대문자를 써야합니다.
