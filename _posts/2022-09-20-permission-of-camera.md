---
title: iOS앱 카메라 사용 권한 요청 쉽고 깔끔하게 하기
date: 2022-09-20 10:31:00 +0900
categories: [iOS, Guide]
tags: [ios, guide, camera, photo, media]
author: WalterCho
---

애플은 아이폰 등 기기가 카메라, 앨범등에 접근하기 위해서 사용자의 동의를 얻도록 강제하고 있습니다. 이유는 간단합니다. 카메라나 앨범이 개인정보에 포함될 수 있기 때문이지요. 그래서 앱 또는 시스템에서 함부로 쓸 수 없도록 강제하고 있는데요, 때문에 개발자는 사용할 수 있는 권한을 달라고 사용자에게 요청합니다.<br>
이때 애플은 기능을 사용하기 바로 직전에 요청하도록 권장하고 있어요.<br>
가끔 앱 시작과 동시에 수많은 권한을 요청하는 것이 리젝트되는 사유가 되기도 하거든요. 그러니 가급적 해당 기능을 사용하려 할 때 요청하도록 하는게 좋겠죠?

## info.plist에 필요한 권한 설정
어떤 권한이든 가장 먼저 해줘야 할 작업입니다. 바로 info.plist에서 요청시 전달할 메세지를 작성하는 것인데요, 카메라 권한은 아래와 같이 `Privacy - Camera Usage Description`을 추가해서 메세지를 작성해주세요.
![Setting info.plist](/post_img/20220920/in_info_plist.png)
_info.plist에서 권한 요청 메세지 작성하기_

그리고 이제 ViewController로 돌아와 viewDidLoad() 또는 버튼 이벤트등 기능이 작동하기 직전에 아래 코드로 사용자에게 권한을 요청하고 응답에 대한 처리를 할 수 있습니다.<br>
`enum AVAuthorizationStatus`의 Int형 상태값을 리턴합니다.
```swift
switch AVCaptureDevice.authorizationStatus(for: .video) {
    case .authorized:
    //요청 허가되었을 때
    //TO DO
        break
    case .denied:
    //요청 거부되었을 때
    //TO Do
        break
    case .restricted:
    //권한이 필요한 장치를 찾을 수 없는 상태
        break
    case .notDetermined:
    //아직 어떤 응답도 없는 상태
        break
}
```

기본적으로 4가지 응답에 대한 처리를 할 수 있지만, 사실상 3가지 경우에 대해서만 처리해도 무방합니다.
```swift
switch AVCaptureDevice.authorizationStatus(for: .video) {
    case .authorized:
        //요청 허가되었을 때 TO DO
        break
    case .notDetermined:
        //아직 응답이 없을 때 TO DO
        break
    default:
        //요청이 거부되었을 때
        //.denied와 .restricted 두 상태 모두 권한 없음으로 처리할 수 있다.
        break
}
```

권한을 요청하는 것은 이것으로 끝입니다!<br>
하지만, 사용자가 권한을 허락하지 않았다면 재요청하는 과정이 반드시 있어야 하겠죠?

## 거부시 재요청하기
보통 카메라를 사용하기 위해 버튼을 눌렀을 때 권한을 요청하곤 합니다.<br>
이때 사용자가 실수로 '거부'를 눌렀다면 어떻게 다시 권한을 요청해야 할까요?

- 버튼을 눌렀을 때 다시 요청한다.
- 아이폰의 [설정]으로 이동해서 권한을 수정할 수 있도록 유도한다.

대충 이정도 방법이 떠오르는데요, 보통 2번째 방법을 가장 많이 쓰고 개인적으로도 추천하는 방법입니다.<br>
이를 위한 샘플코드로는 애플 AVCam프로젝트를 보는 것을 추천드려요. 다른 예시 필요없이 가장 깔끔한 코드가 아닌가 싶네요. `AVCam프로젝트`는 권한 요청과 확인, 거부시 재요청까지 구현되어 있습니다.

한번 만들어 볼까요?<br>
어렵지 않으니 천천히 따라오세요~<br>
일단 아래와 같이 권한 상태에 대한 플래그 변수와 쓰레드를 만들어주세요.
```swift
private let sessionQueue = DispatchQueue(label: "session queue")
private var setupResult: SessionSetupResult = .success
enum SessionSetupResult {
    case success
    case notAuthorized
    case configurationFailed
}
```

상황에 따라 다르겠지만, 만약 앱이 처음부터 카메라 사용을 필요로 한다면 `viewDidLoad()`에서 아래와 같이 요청할 수 있습니다.

```swift
func viewDidLoad() {
    .
    .
    .
    switch AVCaptureDevice.authorizationStatus(for: .video) {
        case .authorized:
            //setUpResult의 초기값이 .success로 되어 있기 때문에
            //권한이 부여된 상태에서는 별다른 처리를 할 필요가 없습니다.
            break

        case .notDetermined:
            //응답이 아직 없는 상태라면,
            //일단 카메라 설정등을 위한 쓰레드를 멈추고
            //권한을 재요청 후 권한을 받았을 때, 쓰레드를 다시 작동시킨다.
            sessionQueue.suspend()
            AVCaptureDevice.requestAccess(for: .video, completionHandler: { granted in
                if !granted {
                    //setUpResult에 아직 거부된 상태임을 저장한다.
                    self.setupResult = .notAuthorized
                }
                self.sessionQueue.resume()
            })

        default:
            //setupResult에 거부된 상태임을 저장한다.
            setupResult = .notAuthorized
    }

    sessionQueue.async {
        //쓰레드가 작동중인 상태라면
        //CaptureSession등을 이때 설정한다.
    }
}
```

 그리고 `viewWillAppear()`에서 상태값 확인 후 그에 따른 로직을 구현하시면 됩니다.<br>

```swift
//viewWillAppear()
sessionQueue.async {
    switch self.setupResult {
    case .success:
        //승인된 상태
        //카메라를 실행합니다.

    case .notAuthorized:
        //거부된 상태
        DispatchQueue.main.async {
            //Alert를 띄워 메세지를 보여주고,
            //UIApplication.shared.open(URL(string: UIApplication.openSettingsURLString)!
            //위 함수를 호출해서 아이폰의 환경설정으로 연결해서 권한을 부여할 수 있습니다.
        }

    case .configurationFailed:
        //상태가 .restricted 등일 때의 오류가 발생한 경우에 대한 처리를 합니다.
        DispatchQueue.main.async {
            //에러 Alert를 띄워 사용자에게 알립니다.
        }
    }
}
```

더 자세한 내용을 보고 싶다면 아래 애플 개발자 사이트를 참고해주세요!<br>
카메라에 대한 샘플코드 또한 있으니 훑어보시면 좋습니다.
<https://developer.apple.com/documentation/avfoundation/capture_setup/avcam_building_a_camera_app>

 그럼, It's coding time~!

 ## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.