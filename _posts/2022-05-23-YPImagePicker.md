---
title: YPImagePicker 적용해보기
date: 2022-05-23 22:10:00 +09:00
tags: iOS swift study YPImagePicker library
categories: [ iOS ]
---

## 설치

CocoaPods를 사용하여 라이브러리를 프로젝트에 추가합니다.

- Project 폴더 내의 Podfile에 `pod YPImagePicker` 추가

```swift
// Podfile

# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'ProjectName' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

  pod 'YPImagePicker'      // Add this!

  # Pods for ProjectName

end
```

- 저장 후 `pod install`
- 카메라, 갤러리 접근 권한 요청 시 나타날 문구 설정(설정 하지 않으면 추후 앱 심사시 통과되지 않을 수 있음)
    - `info.plist` 에 `Privacy - Photo Library UsageDescription` (갤러리 접근 이유), `Privacy - Camera Usage Description` (카메라 접근 이유) 속성을 추가하고 원하는 문구를 작성해줍니다.
    
    ![스크린샷 2022-05-19 14 49 12](https://user-images.githubusercontent.com/40792935/177807710-e22bdece-324b-45c1-9329-30ebb5678f0e.png)

    
- YPImagePicker 호출
    - 더 다양한 상황, 기능 별 샘플 코드는 [YPImagePicker github](https://github.com/Yummypets/YPImagePicker#configuration)에서 확인할 수 있습니다.
    - 아래는 직접 적용해본 코드
    
    ```swift
    @objc func photoAddButtonDidTap(_ sender: UIButton) {
        var config = YPImagePickerConfiguration()
        config.library.maxNumberOfItems = 5 // 최대 선택 가능한 사진 개수 제한
        config.library.mediaType = .photo // 미디어타입(사진, 사진/동영상, 동영상)
    
        let picker = YPImagePicker(configuration: config)
    
        picker.didFinishPicking { [unowned picker] items, _ in
          if let photo = items.singlePhoto {
                print(photo.fromCamera) // Image source (camera or library)
                print(photo.image) // Final image selected by the user
                print(photo.originalImage) // original image selected by the user, unfiltered
                print(photo.modifiedImage) // Transformed image, can be nil
                print(photo.exifMeta) // Print exif meta data of original image.
            }
            picker.dismiss(animated: true, completion: nil) 
        }
        present(picker, animated: true, completion: nil)
      }
    }
    ```
    

![화면 기록 2022-05-23 22 23 04 mov](https://user-images.githubusercontent.com/40792935/169830428-c92c5a64-0d9e-4921-88c4-bfd30f1a0745.gif)

### Reference
[[ios - Swift] 카메라, 앨범 권한 설정](https://poky-develop.tistory.com/20)
