---
title: iOS앱 카메라 권한 요청? 간단하게 만들기
date: 2022-09-20 10:31:00 +0900
categories: [iOS, Guide]
tags: [ios, guide, camera, photo, media]
author: WalterCho
---

애플은 카메라, 앨범등에 접근하기 위해서 사용자의 동의를 얻도록 강제하고 있다.
해당 기능을 사용하기 바로 직전에 요청해야 한다.
가끔 앱 시작과 동시에 수많은 권한을 요청하는 것이 리젝트되는 사유가 되기도 한다.
그러니 기능 사용 직전에 요청하도록 하자.

## 권한
info.plist에서 필요한 권한을 작성할 수 있다.<br>
![Setting info.plist](/post_img/20220920/in_info_plist.png)
_info.plist에서 권한 요청 메세지 작성하기_

```swift
switch AVCaptureDevice.authorizationStatus(for: .video) {
    //권한 상태 확인
}
```

info.plist에서 `Privacy - Camera Usage Description`를 추가하고 권한을 요청할 때 메시지를 작성했다면, AVCaptureDevice.authorizationStatus(for:)를 호출해서 사용자에게 권한을 요청한다.

## 권한 상태
각 권한 상태에 따른 처리를 해주면 된다.<br>
`AVCaptureDevice.authorizationStatus(for: .video)`는 `enum AVAuthorizationStatus`의 Int형 상태값을 리턴한다.

- notDetermined
  : 권한 요청에 대해 아직 아무런 응답이 없는 상태
- restricted
  : 단말기에서 카메라 등 권한이 필요한 장치를 찾을 수 없는 상태
- denied
  : 사용자가 권한 요청을 거부한 상태
- authorized
  : 사용자가 권한 요청을 승인한 상태

## 거부시 재요청
보통 버튼의 IBAction 또는 카메라 뷰컨트롤러의 viewDidLoad()에서 권한을 요청하기 마련이다. 이때 사용자가 본의 아니게 '거부'를 눌렀다면, 개발자는 해당 기능 실행시 권한을 재확인해야 한다.

이를 위한 샘플코드로 애플 AVCam프로젝트를 보는 것을 추천한다. 다른 예시 필요없이 가장 깔끔한 코드가 아닌가 싶다.<br>
`AVCam프로젝트`는 권한 요청과 확인, 거부시 재요청까지 구현되어 있다. 자세하게 보고 싶다면 가장 하단에 링크를 걸었으니 참고하길 바란다.
```swift
//viewDidLoad()
switch AVCaptureDevice.authorizationStatus(for: .video) {
    case .authorized:
        //권한이 부여된 상태에서는 아무것도 하지 않는다.
        break

    case .notDetermined:
        sessionQueue.suspend()
        AVCaptureDevice.requestAccess(for: .video, completionHandler: { granted in
            if !granted {
                //
                self.setupResult = .notAuthorized
            }
            self.sessionQueue.resume()
        })

    default:
        //거부된 상태에서는 전역변수 setupResult에 그 상태를 저장한다.
        setupResult = .notAuthorized
    }
```

이렇게 viewDidLoad()에서 호출해주면 카메라 뷰컨트롤러가 실행되는 즉시 info.plist에서 작성한 권한 요청 메세지가 나타나는 것을 볼 수 있다. 

이후 뷰컨트롤러가 화면에 나타날때 호출되는 `viewWillAppear()`에서 setUpResult상태를 확인, 거부된 상태일 때 재요청할 수 있다!

```swift
//viewWillAppear()
sessionQueue.async {
    switch self.setupResult {
    case .success:
        //승인된 상태
        //captureSession을 실행하고, 카메라를 사용한다.

    case .notAuthorized:
        //거부된 상태
        DispatchQueue.main.async {
            //Alert등을 띄워 메세지를 보여주고,
            //UIApplication.shared.open(URL(string: UIApplication.openSettingsURLString)!
            //위 함수를 호출해서 아이폰의 환경설정으로 연결해서 권한을 부여할 수 있다.
        }

    case .configurationFailed:
        //권한 상태가 .restricted 등일 때의 오류가 발생한 경우다.
        DispatchQueue.main.async {
            //에러 Alert등을 띄워 사용자에게 알린다.
        }
    }
}
```

애플 AVCam프로젝트를 더 자세하게 보러가기
<https://developer.apple.com/documentation/avfoundation/capture_setup/avcam_building_a_camera_app>


***AVCam프로젝트를 참고하고도 권한 설정에 대해 이해가지 않는 부분이 있다면, 메일을 보내주세요.***

 그럼, It's coding time~!

 ## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.