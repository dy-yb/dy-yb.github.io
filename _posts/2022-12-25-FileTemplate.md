---
title: Xcode File Template
date: 2022-12-25 21:30:01 +09:00
tags: iOS swift study xcode template
categories: [ iOS ]
---

오랜만에 크리스마스 맞이(?) 블로그 글을 작성해봅니다,,

프로젝트에서 VIPER 아키텍처를 채택하여 사용하고있는데, 화면 당 같은 구조의 코드를 가진 파일을 반복해서 생성해야 하는 수고가 있었다. Xcode의 파일 템플릿을 이용하면 이러한 반복작업을 줄일 수 있다고 하여 방법을 찾아 시도해 봤다!


우선 한번쯤 사용해 봤을 Cocoa Touch Class 또한 template으로 만들어져 있는 것인데, 그 구조를 참고하여 수정하면 나만의 템플릿을 생성할 수 있다고 한다.

기존 파일 템플릿이 어떠한 구조로 만들어져 있는지 참고하기 위해서는 다음 경로로 들어가면 된다.

```swift
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/File Templates/
```

(Finder를 통하여 찾고싶은 경우에는 패키지 내용보기로 접근할 수 있다.)

<img width="338" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_20 49 56" src="https://user-images.githubusercontent.com/40792935/209468282-9c37cb6b-7af4-473e-a604-c9668ddcca3d.png">

File Template 기본 구조는 다음과 같다.

**.xctemplate (폴더)**

- **TemplateIcon.png**
    - File template 이미지 아이콘
- **TemplateInfo.plist**
    - template에 관한 정보
- **\_\_\_ FILEBASENAME\_\_\_.swift**
    - 최종적으로 생성되는 파일

위 세가지 파일을 xxxx.xctemplate 형태의 이름을 가진 폴더에 넣어 올바른 위치에 생성하면 되는데, 아이콘 이미지는 기존 템플릿에서 사용하고 있는 아이콘 이미지를 복사하여 사용해도 무방하다.

### custom file template 생성하는 법

- .xctemplate 폴더 만들기
    
    ```swift
    ~/Library/Developer/Xcode/Templates/AAAAA/BBBBB.xctemplate
    ```
    
    - ~/Library/Developer/Xcode/Templates/ 아래에 폴더를 생성하고, 해당 폴더 내에 .xctemplate을 생성하면된다.
    - 이때 상위 폴더 이름(AAAAA)이 아래 사진처럼 파일 생성 인터페이스에서 헤더 부분으로 나타나게 됨!
        - ~/Library/Developer/Xcode/Templates/Custom\ Templates/KnockKnock-VIPER.xctemplate
        
        ![Untitled](https://user-images.githubusercontent.com/40792935/209468301-3850248e-6370-46d0-adc0-2d8de2a70905.png)
        
        

- .xctemplate 폴더를 만들었으면, 해당 폴더 안에 기존 파일 템플릿 파일들을 복사 붙여넣기 해준다.
  <img width="298" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_21 09 01" src="https://user-images.githubusercontent.com/40792935/209468287-15ac2f8b-f61f-4665-bd20-3a4f941da0ad.png">
    
- **TemplateInfo.plist** 파일을 수정한다.
    - plist파일에서는 템플릿에대한 여러가지 옵션을 부여할 수 있다.
        ![U1](https://user-images.githubusercontent.com/40792935/209468298-2c85f802-894f-4245-a59f-e3b184d0a51d.png)
        
    - Kind: 템플릿 종류
    - Description, Summary: 템플릿 설명
    - SortOrder: 템플릿 정렬 순서
    - BuildableType: 해당 값은 템플릿을 통해 파일 생성시 Targets이 test로 되어있도록 하는 디폴트 설정 값
    - DefaultComletionName: 해당 template file을 만들때 미리 입력되어 있는 이름
    - Option: 템플릿 생성 시 사용자로 부터 입력받을 내용 (아래 사진 부분)
        
    <img width="723" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_21 14 57" src="https://user-images.githubusercontent.com/40792935/209468288-2fcc93c4-71ec-4671-b678-2b61c390d118.png">
        
    
- 이제 **\_\_\_ FILEBASENAME\_\_\_.swift** 파일을 원하는대로 수정해주면 된다.(언더바 앞 뒤로 세개씩!)
    - //\_\_\_FILEHEADER\_\_\_
        - 파일 생성시 자동으로 생성되는 파일의 정보를 담는 주석 부분
        - 코드 예
        
        ```swift
        //
        //  ViewController.swift
        //  KnockKnock-iOS
        //
        //  Created by Daye on 2022/02/13.
        //
        ```
        
    - \_\_\_VARIABLE\_productName (언더바 3개 1개)
        - 파일 생성 시 입력한 내용으로 치환 됨
        - 다음 화면에서 test로 입력한다면, \_\_\_VARIABLE\_productName 부분은 test로 치환
          <img width="731" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_21 24 21" src="https://user-images.githubusercontent.com/40792935/209468294-eaeafb2f-b8bb-41bc-9348-d58b1cd21858.png">
        - (해당 화면에 대한 설정은 **TemplateInfo.plist** 에서 할 수 있음)
    - \_\_\_FILEBASENAMEASIDENTIFIER\_\_\_
        - 파일 이름
        - 예를 들어, 파일의 이름을 **\_\_\_ FILEBASENAME\_\_\_ViewController.swift** 로 지정하면, 해당 부분은 testViewController로 치환 된다.

다음 코드는 내가 위 3가지를 적용하여 ViewController 파일에 대한 템플릿을 작성한 것이다.

- **\_\_\_ FILEBASENAME\_\_\_ViewController.swift**

```swift
//___FILEHEADER___

import UIKit

protocol ___VARIABLE_productName:identifier___ViewProtocol: AnyObject {
  var interactor: ___VARIABLE_productName:identifier___InteractorProtocol? { get set}
}

final class ___FILEBASENAMEASIDENTIFIER___: BaseViewController<___VARIABLE_productName:identifier___View> {

  // MARK: - Properties

  var interactor: ___VARIABLE_productName:identifier___InteractorProtocol?

  // MARK: - Life Cycle

 
  override func viewDidLoad() {
    super.viewDidLoad()

  }

  override func setupConfigure() {

  }
}

// MARK: - ___VARIABLE_productName:identifier___ View Protocol

extension ___FILEBASENAMEASIDENTIFIER___: ___VARIABLE_productName:identifier___ViewProtocol {

}
```

이렇게 ViewController 뿐만아니라 Interactor, Presenter, Worker, Router 등 프로젝트에서 기본적으로 사용하는 파일들에 대하여 템플릿을 생성하여, 필요한 파일들을 손쉽게 자동으로 생성할 수 있었다. 

- 적용 화면
<img width="417" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_21 21 41" src="https://user-images.githubusercontent.com/40792935/209468289-fdbde309-587c-46d5-8e74-cf067cabbf5c.png">
 
<img width="731" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_21 24 21" src="https://user-images.githubusercontent.com/40792935/209468294-eaeafb2f-b8bb-41bc-9348-d58b1cd21858.png">
        
<img width="298" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_21 23 32" src="https://user-images.githubusercontent.com/40792935/209468290-baa383b5-dd47-4eb5-a654-62e91cad234d.png">
        
<img width="744" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-12-25_21 23 48" src="https://user-images.githubusercontent.com/40792935/209468292-7805cb48-9bdc-4931-a030-97dea15ba629.png">




### 참고

[[iOS] Xcode File Template을 이용해 반복작업 줄이기](https://tech.lezhin.com/2019/12/16/ios-file-template)

[[iOS - swift] Xcode 코드 템플릿 (File Template, Custom File Template)](https://ios-development.tistory.com/760)
