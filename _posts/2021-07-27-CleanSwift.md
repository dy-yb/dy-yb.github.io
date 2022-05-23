---
title: Clean Swift(VIP)의 장점 및 타 아키텍쳐의 한계점
date: 2021-07-19 15:52:00
tags: swift study vip architecture
categories: [ iOS ]
---

[https://clean-swift.com/handbook/](https://clean-swift.com/handbook/) 의 번역입니다. 의역/오역이 있을 수 있습니다. 임의로 생략한 내용이 있습니다.

*Clean Swift는 템플릿을 다운 받으면 편리하게 작성이 가능합니다.([Clean Swift Template Download](https://medium.com/swift2go/installing-the-clean-swift-template-in-xcode-6b4367006827))*

---

## 도대체 MVC는 왜 인기가 많을까?

iphone이 나온지 10년 정도 되었고 iOS 개발은 이제 더이상 새로운 것이 아니죠. 그동안 MVC는 항상 문제가 있었음에도 불구하고 전형적인 xcode 프로젝트에서 가장 인기 있는 구조입니다.

애플의 샘플코드들은 MVC를 사용하고 있습니다. 새로운 프레임워크를 배우려할 때, 애플의 샘플 코드를 통해 어떻게 새로운 api를 호출는지 배울 수 있죠. MVC는 기능들을 손쉽게 보여줄 수 있습니다. 한번 이렇게 수행하고나면, 아마 계속 이 방법에 머무르게 됩니다. 그리고 다음 기능으로 넘어가겠죠? 그러고 나면 데드라인에 가까워질겁니다.

왜 Apple이 모든 것을 View controller에 작성하는지에 대해 생각해본적 있나요? 그 이유는 가장 쉬운 방법으로 새로운 API들을 보여주기 위해서입니다. 그 목적은 아주 잘 달성됐고요. 하지만 우리가 도태되는 것에 대해서는 말해주지 않습니다. Apple은 단지 샘플 프로젝트의 시작점이 무엇인지 보여준 것 뿐입니다. 

MVC를 주로 사용하게되는 또 다른 이유는 당신이 MVC 프로젝트를 상속받았기 때문입니다. 프로젝트가 mvc로 시작되면 mvc에 머물고 mvc로 죽게되죠. 이는 코드가 종양이 아주 커져버려 이미 4기 암 단계에 있는 것과 같은 최악의 상황입니다. 

## MVVM/VIPER에 대해

분명 다른 아키텍쳐에 대해서도 들어본적이 있을겁니다. 이 문제들이 진짜 현존하기 때문이죠.  MVC는 iOS 개발에 맞지않습니다. MVVM와 VIPER를 써보셨다면, 여기에도 또 그들만의 문제가 있다는걸 알고 계실겁니다.

MVVM를 사용하면 View controller로 부터 View models로 많은 것들을 간단하게 옮길 수 있습니다. 복잡한 View controller들 대신에 복잡한 View model을 갖게되죠.

MVVM의 인기는 반응형 외부 종속 라이브러리(ReactiveCocoa/Rxswift)때문이라 해도 과언이 아니죠. 근데 사실 대부분 반응형은 필요없습니다. 심지어 실시간 채팅 기능 개발에서도...

~~VIPER는 Clean 아키텍쳐를와 가장 근접하게 사용할 수 있습니다. 매우 중요한 소프트웨어 시스템에서 사용하면서 오랜 시간 증명되었습니다.~~  이는 자바와 비슷한 많은 언어에서 사용되었고, 많은 뛰어난 오래된 소프트웨어 디자인 정책에서 사용됐습니다. 

그치만 아주 중요한 것을 놓쳤습니다. VIPER는 구조의 중심에 presenter를 두고있으면서 사용자 동작과 모델 업데이트, 그리고 라우팅까지 모두 presenter를 통해 지나가도록 합니다. 이 때문에 presenter는 scene내에 다른 모든 요소들의 레퍼런스를 갖게됩니다. 결과적으로 거대하고 복잡한 presenter를 갖게 되겠죠.

## Clean Swift를 통해 iOS 앱을 체계적으로 개발하는 법

따를 수 있는 시스템이 있다면 어떨 것 같으세요? ~~이 세상에서 복잡한 view controller 문제를 무너트리고 앱 코드를 작성하는 것 보다 쉽게 unit test를 작성하고 유지보수하도록 만들 수 있습니다.~~

더 이상 즉석에서 기능을 향상시키거나 스파게티 코드를 작성할 필요가 없습니다. 이해할 수 있는, 읽을 수 있는 코드만 남기게 할 수 있습니다. unit test에 아주 견고하기떄문에 몇 주, 몇 달, 몇 년이 지나도 자신있게 앱을 수정할 수 있습니다. 

시스템이 왜 리팩토링보다 좋을까요?

증명된 시스템은 미리 결론지을 수 있게 만들어줍니다. 끊임없는 리팩토링에 시간을 낭비하지 않게 해줍니다. 

## 시스템 둘러보기

Clean Swift는 VIPER가 갖고있는 흐름제어 문제를 VIP 사이클을 통해 해결합니다. VIP 사이클은 단방향 흐름제어를 제공하고 시스템의 기초를 만들어줍니다. 

새로운 기능을 구현할 때는, 다음과 같은 단계를 따라 진행하게 됩니다:

1. IBAction 메소드 또는 viewDidLoad()로 부터 유스케이스를 트리거 함
2. view controller가 interactor를 호출하고, interactor는 일부 비즈니스 로직을 수행
3. interactor는 presenter를 호출하고, 결과를 Primitive Type으로 형식화
4. Presenter는 View Controller를 호출하고, View controller는 결과를 화면에 표시

흐름제어는 항상 다음과 같이 됩니다:

![VIP_flow](https://user-images.githubusercontent.com/40792935/153118111-026a83ae-eb93-4b3a-b2ff-257ee43a7c99.png)

반응형에 대해서는 어떨까요? Clean Swift에서는 Reactive Cocoa나 RxSwift가 필요없습니다. 시그널에 대해서도 배울 필요가 없습니다. VIP 사이클에서 interactor내의 클로저 기반의 비동기 메소드가 주기적인 업데이트를 가능하게 합니다.

장점이 또 있습니다:

- 복잡한 비즈니스 로직은 interactor가 worker를 호출하여 수행할 수 있습니다.
- 새롭게 개선 된 라우팅 시스템은 라우팅 프로세스를 navigation과 data passing 두 단계로 분리하여 수행합니다.
- request, reponse, and view model은 데이터의 독립성을 제공 → unit test를 작성할 때 아주 중요한 역할을 수행합니다.

---

Reference: [https://clean-swift.com/handbook/](https://clean-swift.com/handbook/), [https://velog.io/@ssionii/Clean-Swift-a.k.a-VIP-에-대해-아라보자](https://velog.io/@ssionii/Clean-Swift-a.k.a-VIP-%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%84%EB%9D%BC%EB%B3%B4%EC%9E%90)
