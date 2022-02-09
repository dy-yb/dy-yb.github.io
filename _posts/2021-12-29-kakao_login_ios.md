---
title: iOS + Kakao login 사용하기
date: 2021-07-28 19:39:00
tags: xcode swift ios study kakaosdk kakaologin
---

-

# Kakao social login

[Kakao Developers](https://developers.kakao.com/docs/latest/ko/kakaologin/ios)

카카오 로그인은 사용자를 인증하고 토큰을 발급하는 기능이다.

### 토큰 종류

- 액세스 토큰
    - 사용자 정보 기반의 API call
- 리프레시 토큰
    - 일정기간 동안 사용자 인증 절차를 거치지 않아도 액세스 토큰을 갱신 하도록 해줌

### 토큰 발행 절차

1. 카카오 계정 인증
    1. 카카오톡으로 연동하여 로그인
    2. 웹브라우저에 계정 정보 입력을 통한 로그인
2. 사용자 동의(이미 동의한 경우 스킵)
3. 인가 코드 발급
4. 인가 코드를 통해 토큰 발급

---

### 1. Cocoapods를 이용하여 SDK 설치

- SDK전체를 설치하고 싶다면
    - KakaoSDK
    
    ```bash
    target 'Project_Name' do
      # Comment the next line if you don't want to use dynamic frameworks
      use_frameworks!
    	pod 'KakaoSDKCommon'
    
      # Pods for Project_Name
    
    end
    ```
    
- 로그인 구현에 필요한 모듈만 설치하려면
    - KakaoSDKCommon, KakaoSDKAuth, KakaoSDKUser
    
    ```bash
    # Uncomment the next line to define a global platform for your project
    # platform :ios, '9.0'
    
    target 'Project_Name' do
      # Comment the next line if you don't want to use dynamic frameworks
      use_frameworks!
    	pod 'KakaoSDKCommon'
    	pod 'KakaoSDKAuth'
    	pod 'KakaoSDKUser'
      # Pods for Project_Name
    
    end
    ```
    

### 2. Info.plist 설정

- 앱 실행 허용 목록(Allowlist) 등록하기
    - Info > Custom iOS Target Properties > `LSApplicationQueriesSchemes` (type: Array) 추가 후 item 추가
        - 만들어져 있는 항목이 없기 때문에 직접 추가해야함!
        - item0: kakaokompassauth
        - item1: kakaolink
- URL Schemes 설정
- Info > URL Types > URL Schemes 항목에 네이티브 앱 키(Native App Key)를 입력
    - 'kakao{NATIVE_APP_KEY}'
        - ex: native app key가 ‘1124234’ 이면, kakao1124234를 입력

### 3. 초기화

`AppDelegate.swift`

```swift
import KakaoSDKCommon

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
  KakaoSDK.initSDK(appKey: "{NATIVE_APP_KEY}")

	return true
}
```
