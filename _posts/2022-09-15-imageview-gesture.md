---
title: Swift iOS 뷰 또는 이미지 뷰에 터치 이벤트 주기!
date: 2022-09-15 19:42:00 +0900
categories: [iOS, Guide]
tags: [ios, guide]     # TAG names should always be lowercase
author: WalterCho
---

Button은 IBAction을 **직접**연결해서 터치 이벤트를 처리할 수가 있다.<br>
그럼, UIView는? ImageView는? Label은?

우린 종종 View 또는 ImageView, Label 등에서도 터치 이벤트를 처리해야할 때가 있다.<br>
간단하게 제스처(Gesture)를 연결해주면 된다.

## 뷰에 TapGestureRecogniger 연결하기
각각 UIView, UIImageView, UILabel에 연결을 해볼텐데, ***ImageView와 Label은 조금 다른 점이 있으니 잘 살펴 보자.***
```swift
//탭 제스처를 만든다.
let gesture = UITapGestureRecognizer(
                target: self, 
                action: #selector(self.viewTapped(_:))
            )
```

이 제스처는 action파라미터인 #selector로 선언된 함수를 호출하며, 이 함수는 Objective-C의 형태로 만들어져야 한다.<br>
@objc 어노테이션을 함수 앞에 붙여주면 Objective-C 타입의 함수로 만들 수 있다.
```swift
@objc
private func viewTapped(_ sender: UITapGestureRecognizer) {
    self.updateUI(text: "뷰가 눌렸습니다!")
}
```

@objc는 Swift가 나오기 전 애플 공식 개발 언어였던 Objective-C코드를 Swift에서 사용하기 위해 만들어진 어노테이션이다.<br>
다시말해, `UITapGestureRecognizer()`는 Objective-C 형태의 함수 코드를 필요로 하는데, Swift코드에서도 쓸 수 있도록 붙여주는 것이라고 생각하면 된다!

이 제스처가 어떤 동작을 할 것인지 함수까지 만들었다면, 이제 아래와 같이 뷰에 연결만 해주면 된다.
```swift
//뷰에 제스처를 연결한다.
self.uiView.addGestureRecognizer(gesture)
```

## isUserInteractiveEnabled 설정하기
ImageView 또는 Label 역시 이와 같은 작업을 해주면 되는데, UIView와는 다르게 한가지 작업이 더 필요하다.
```swift
let gestureForLabel = UITapGestureRecognizer(
                        target: self, 
                        action: #selector(self.labelTapped(_:))
                    )
self.label.isUserInteractionEnabled = true              //바로 이것!
self.label.addGestureRecognizer(gestureForLabel)
```

UIView는 기본적으로 사용자와 상호작용할 수 있도록 isUserInteractiveEnabled가 true로 설정되어 있다.<br>
하지만, ImageView와 Label은 false가 기본값이다. 따라서 true로 바꾸어 줘야 한다.

물론 아래와 같이 스토리보드에서도 설정할 수 있다.
![isUserInteractiveEnabled in Storyboard](/post_img/20220915/isUserInteractiveEnabled_in_storyboard.png){: width="350" }
_스토리보드에서 isUserInteractiveEnabled설정_