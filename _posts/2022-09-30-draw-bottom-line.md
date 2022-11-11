---
title: iOS 뷰의 특정 부분에 테두리 그리기!
date: 2022-09-30 21:25:00 +0900
categories: [iOS, Guide]
tags: [ios, guide]
author: WalterCho
---

스토리보드 또는 SwiftUI로 View를 그리다보면 종종 테두리가 필요할 때가 있다.

## 전체 테두리 그리기
대게는 이렇게 전체 테두리를 그린다.<br>
![round of camera](/post_img/20220930/camera_border.png){: width="70"}

이미 알고 있겠지만, 코드는 이렇다.
```swift
self.cameraBtn.layer.borderColor = UIColor.black.cgColor
self.cameraBtn.layer.borderWidth = 1.0
self.cameraBtn.layer.cornerRadius = 10
self.cameraBtn.layer.masksToBounds = true
```

## 부분 테두리 그리기
하지만 가~끔은 위아래 혹은 좌우 혹은 한쪽만 라인을 그리고 싶을 때가 있다.
Layer를 확장(Extension)해주면 된다.

```swift
extension CALayer {
    func drawLineAt(edges:[UIRectEdge], color: UIColor, width: CGFloat) {
        for edge in edges {
            let border = CALayer()
            
            switch edge {
            case UIRectEdge.top:
                border.frame = CGRect.init(
                    x: 0,
                    y: 0,
                    width: bounds.width,
                    height: width
                )
            case UIRectEdge.bottom:
                border.frame = CGRect.init(
                    x: 0,
                    y: frame.height-width,
                    width: bounds.width,
                    height: width
                )
            case UIRectEdge.left:
                border.frame = CGRect.init(
                    x: 0,
                    y: 0,
                    width: width,
                    height: bounds.height
                )
            case UIRectEdge.right:
                border.frame = CGRect.init(
                    x: frame.width-width,
                    y: 0,
                    width: width,
                    height: bounds.height
                )
            default:
                break
            }
            
            border.backgroundColor = color.cgColor
            self.addSublayer(border)
        }
    }
}
```

### 사용 예시
TextView, TextField, Label을 만들어 각각 Top, left/right, bottom을 줬다.<br>
테두리를 그리고 싶은 부분을 배열로 전달하면 간단하다.
```swift
//textView의 위쪽에만 빨간색으로 테두리를 그린다.
func drawLineAtTopOfTextView() {
    self.textView.layer.drawLineAt(
        edges: [.top],
        color: UIColor.red,
        width: 5.0
    )
}

//textField의 왼쪽과 오른쪽에 갈색으로 테두리를 그린다.    
func drawLineAtLeftAndRightOfTextField() {
    self.textField.layer.drawLineAt(
        edges: [.left, .right],
        color: UIColor.brown,
        width: 5.0
    )
}
    
//label의 아래쪽만 테두리 그리기
func drawLineAtBottomOfLabel() {
    self.label.layer.drawLineAt(
        edges: [.bottom],
        color: UIColor.black,
        width: 5.0
    )
}
```

UIRectEdge는,
- .all
- .top
- .bottom
- .left
- .right
  
총 다섯개의 상태값을 가지고 있다.

### 결과
아래와 같이 지정된 부분에 테두리가 그려지는 것을 확인할 수 있다.
![part border](/post_img/20220930/part_border.png){: width="450"}


## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.