---
title: Xcode Cloud + Firebase App Distribution 자동화 배포
date: 2023-03-18 16:19:00
tags: study firebase xcode cd app distribution
categories: [ iOS ]
---

앱 배포 자동화를 위해서 Xcode Cloud에 Workflow를 생성 및 설정하고 Firebase Distribution을 연결해보았다. 
**(Xcode Cloud를 통해 관리하려는 project가 Github에 반드시 올라가 있어야 가능. 해당 글은 공개 repo를 기준으로 작성되어있으며, 비공개 repo의 경우 별도의 인증 절차가 추가로 필요하다고 함!)**

1. Xcode Cloud 설정
    1. Product > Xcode Cloud > Create Workflow..
    2. Select Product 창에서 앱 선택
        
        <img width="697" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 12 38" src="https://user-images.githubusercontent.com/40792935/226105763-efc3d87a-8f02-4971-af07-ac041d982e09.png">
        
        <img width="700" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 12 11" src="https://user-images.githubusercontent.com/40792935/226105762-46a48c23-6a07-4e49-9a5a-04ae88f7bca0.png">

        <img width="702" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 13 07" src="https://user-images.githubusercontent.com/40792935/226105764-c5372c87-e7d8-459c-b27b-214327a78440.png">        
        
        <img width="474" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 13 54" src="https://user-images.githubusercontent.com/40792935/226105765-bf7de8a0-1a1b-4a79-9015-cc8ad55c0c1a.png">
        
        <img width="684" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 14 24" src="https://user-images.githubusercontent.com/40792935/226105766-583795ce-7974-4af3-b906-4276dc4ab4d1.png">
        
        <img width="725" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 15 17" src="https://user-images.githubusercontent.com/40792935/226105768-aeb2d778-9fdd-4699-919f-ce9d9d97e56b.png">
        
        <img width="610" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 42 12" src="https://user-images.githubusercontent.com/40792935/226105773-7231baf5-ded7-4fb1-b55d-3098b14bdacd.png">
        
        <img width="676" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 15 44" src="https://user-images.githubusercontent.com/40792935/226105769-38843953-d442-4467-b539-f7601cc4c18c.png">
        
        - install 해주고 나서 다시 xcode로 돌아가면 next버튼이 활성화 되어있을 것이다. (클릭)
        - Confirm App on App Store Connect  → Complete 클릭
        - Start Build → branch 선택 → Start Build 클릭
            - default 설정의 build가 수행될 것..
        
    3. Workflow 설정 (설정한 것 들만 note)
        
        <img width="956" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 04 08" src="https://user-images.githubusercontent.com/40792935/226105756-ee84230c-73ae-4591-bc47-4de4099a6a66.png">
        
        - General
            - Name : Workflow 이름
            - Description: Workflow 설정
        
        <img width="958" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 09 16" src="https://user-images.githubusercontent.com/40792935/226105760-1386537b-9dbf-4a53-a526-511321d3c4b4.png">
        
        - Environment
            - Xcode Version, macOS Version 모두 현재 사용하고 있는 버전으로 맞추어주었음
        
        <img width="960" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 26 03" src="https://user-images.githubusercontent.com/40792935/226105772-124e6710-fc4b-42e1-bd20-22184db9aba7.png">
        
        - Start Conditions (실행 될 조건)
            - 작업사항이 `develop` 브랜치에 `merge`되었을 때 실행되도록 했음
            - Options > Auto-cancel Builds를 선택하면 가장 마지막에 요청한 빌드가 실행되고 이전에 진행 중이었던 빌드는 취소 됨
            
            <img width="269" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_11 22 18" src="https://user-images.githubusercontent.com/40792935/226105771-8c37716f-4dd6-4eeb-811f-2c4c4d7ba72a.png">
            
        - Actions
            - 앱 배포 처리를 위해서 Archive 선택
        
        Save 버튼을 눌러 설정을 완료하고, Start Condtitions에 맞는 행동을 수행하면 빌드가 처리되는 것을 볼 수 있을 것이다. 
        
    

### CocoaPod 라이브러리 관련 설정

- .gitignore에서 `Podfile` , `Podfile.lock` 파일을 포함하고 있지 않은지 확인!
- 프로젝트 루트 위치에 `ci_scripts` group 생성
    - ci_post_clone.sh
    
    ```swift
    # Install CocoaPods using Homebrew.
    brew install cocoapods
    
    # Install dependencies you manage with CocoaPods.
    pod install
    ```
    

- 관련 Apple 공식문서
    
    [Apple Developer Documentation](https://developer.apple.com/documentation/xcode/making-dependencies-available-to-xcode-cloud)
    

### Firebase Distribution 연결 설정

- Firebase CLI 설치
    - 다음 링크 접속 후 macOS용 Firebase CLI 바이너리 다운로드
        
        [Firebase CLI reference | Firebase Documentation](https://firebase.google.com/docs/cli#install-cli-mac-linux)
        
    
        ![Untitled](https://user-images.githubusercontent.com/40792935/226177209-0537dd56-2232-4edb-a55b-786fbd7869dc.png)

    
    - 다운받은 파일을 `.zip` 으로 압축하고, `ci_scripts` group에 추가
    - `ci_post_xcodebuild.sh`
        
        ```swift
        if
        then
            echo "Uploading Firebase Debug Build"
            unzip firebase-tools-macos.zip
            chmod +x ./firebase-tools-macos
            ./firebase-tools-macos appdistribution:distribute $CI_AD_HOC_SIGNED_APP_PATH/APP_NAME-.ipa --app FIREBASE_APPID --token $FIREBASE_TOKEN --groups $TESTER_GROUP
        fi
        ```
        
        - APP_NAME: 앱 명
        - FIREBASE_APPID
            - Firebase console > 프로젝트 설정 > 내 앱 > 앱 ID
            - 1:nnnnnnnnn:ios:xxxxxxxxxxxxxxx 형태의 아이디
        - $FIREBASE_TOKEN, $TESTER_GROUP(선택)
            - workflow 설정에서 환경변수 설정해서 사용
            - firebase token 확인하는 방법
                - 터미널 환경에서 `firebase login:ci` 실행 후 로그인하면 토큰 값 확인 가능
            - tester group
                - Firebase Distribution tester group 설정 후 group name 적으면 됨
    - 관련 Apple 공식문서
        - Script 작성 시 활용할 수 있는 여러 환경변수들이 설명되어있다.
        
        [Apple Developer Documentation](https://developer.apple.com/documentation/xcode/environment-variable-reference)
        
    - 참고 글
        
        [Using Xcode Cloud With Firebase](https://medium.com/1v1me-blog/using-xcode-cloud-with-firebase-3bc66e7094bb)
        
### Slack notify 연결

기본적으로 빌드 성공 여부를 이메일로 받을 수 있지만, notify 설정을 통해 Slack으로도 받을 수 있다.

Xcode Cloud Workflow 설정 페이지에서 하는 방법, App Store Connect 페이지에서 하는 방법이 있는데 이상하게 전자는 잘 되지 않아서 후자 방식으로 진행하였다.

- App Store Connect > Xcode Cloud > 워크플로 관리 > Workflow 선택 > 사후작업 > Slack 편집
    
    <img width="1453" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2023-01-09_12 04 52" src="https://user-images.githubusercontent.com/40792935/226177265-d2ae4bfa-c5e9-425d-9b7b-738c42a1e269.png">
    
- Slack 주소 입력 및 계정 로그인 후 연결하려는 채널 선택 후 저장

- 관련 Apple 공식문서
    
    [Apple Developer Documentation](https://developer.apple.com/documentation/xcode/connecting-xcode-cloud-to-slack)

        
