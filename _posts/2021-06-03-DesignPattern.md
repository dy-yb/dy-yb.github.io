---
title: Design Pattern
date: 2021-06-03 19:59:00
tags: study swift iOS
categories: [ iOS ]
---

## 왜 아키텍처를 고려해야 할까?

아키텍처를 고려하지 않으면, 여러 객체가 담겨 거대하게 커져버린 클래스를 디버깅 할 때 어떠한 것도 찾을 수도 없고 고칠 수도 없게 됩니다. 

지금 여러분의 상황이 이렇진 않나요?:

- 클래스는 UIViewController의 서브 클래스이다.
- 데이터는 UIViewController에 직접 저장된다.
- UIView들은 아무것도 하지 않는다.
- Model은 바보같은 데이터 구조로 되어있다.
- Unit test는 아무것도 커버하지 못한다.

애플의 가이드라인을 따르고 MVC패턴을 사용하더라도 이런 일은 벌어질 수 있습니다. 애플의 MVC에는 뭔가 잘못 된것이 있습니다. 그렇다면 **좋은 아키텍처**란 무엇일까요?

- **Distribution** - 객체들의 역할을 정확한 룰에 따라 균형있게 분배되었는가?
    - Single-responsibility principle
        - 하나의 객체는 한 가지의 역할만 가짐 (=역할이 분명해야한다.)
- **Testability** - 테스트가 가능한가?
    - 복잡한 리팩토링을 진행하거나, 새로운 기능을 추가할 때 쉬워짐
    - 개발자가 런타임 내에 문제를 찾지 못하게 되어 출시 이후에 발견되면, 수정되기까지 일주일 소요(앱스토어 리뷰 필요)
- **Ease of use** - 사용하기 쉬운가?
    - 설명이 필요없는 간단명료한 코드작성은 유지보수 비용을 절감시킴

## MV(X) essentials

요즘에는 아키텍처 디자인 패턴에 다양한 옵션이 있습니다(MVC, MVP, MVVM, VIPER)

MVC, MVP, MVVM은 다음 3가지 항목을 가지고 있습니다.

- Models
    - 애플리케이션에서 사용할 데이터를 관리하는 부분
- Views
    - 표현계층(GUI), iOS 환경에서 'UI'가 접두사로 붙은 모든 것들이 해당 됨
- Controller/Presenter/ViewModel
    - Model과 View를 연결하는 부분
    - 일반적으로 View에서 사용자의 동작/요청에 반응하여 Model을 변경하고 Model의 결과로 부터 View를 업데이트함

---
- References
    - https://medium.com/ios-os-x-development/ios-architecture-patterns-ecba4c38de52
    - https://jiyeonlab.tistory.com/37?category=818842
    - http://labs.brandi.co.kr/2018/02/21/kimjh.html
