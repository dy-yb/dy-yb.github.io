---
title: 3rd-party library 추가하기
date: 2021-07-28 19:39:00
tags: xcode swift ios study cocoapods
categories: [ iOS ]
---

1. **Xcode에서 추가**
    1. File -> Swift Packages -> Add Package Dependency 를 클릭 → git url 복사 → 끝

2. **CocoaPods 사용**
    1. cocoapods 설치 (Terminal)

        ```bash
        sudo gem install cocoapods
        ```

    2. 프로젝트 폴더로 이동 후 initialization

        ```bash
        cd path
        pod init
        ```

    3. 프로젝트 폴더에 생성 된 Podfile 열기

        ```bash
        vi podfile
        ```
![terminal-screenshot](https://user-images.githubusercontent.com/40792935/153118151-4846fe4e-1420-4944-8234-0eef0ea8cbbc.png)

        ```bash
        # Uncomment the next line to define a global platform for your project
        # platform :ios, '9.0'

        target 'Address-Finder' do
          pod 'Alamofire', '~> 4.4'
          # Comment the next line if you don't want to use dynamic frameworks
          use_frameworks!

          # Pods for Address-Finder

        end
        ```

        - target 'project-name' do 아래에 설치하고 싶은 라이브러리 이름과 버전 작성

        d. 설치

        ```bash
        pod install
        ```

        e. 설치가 끝나면, 폴더에 생성 된 projectName.**xcworkspace** 오픈

        f. 라이브러리 import 

        ```bash
        import UIKit
        import Alamorfire
        ```
