---
title: iOS, UIKit 제스쳐 알아보기 1편 - UIPanGesture
date: 2022-10-26 22:52:00 +0900
categories: [iOS, Util]
tags: [ios, UiKit, Gesture]
author: JohnCoder
---

우리가 스마트폰을 쓸 때 화면을 터치를 하거나, 드래그 혹은 두 손가락으로 사진을 확대하는 동작을 <br>
가장 많이 사용하는 것 같다.
이번 포스팅에서는 여러가지 동작 중 화면을 드래그 하는 동작을 받아서 뷰를 동작시키는 것을 알아보자.


![Kind of Gesture in UIKit](/post_img/20221026/Gesture.png)
_제스처의 종류_
xcode의 스토리보드 상에서 "+" 버튼을 눌러서 gesture를 검색하면 위 사진처럼 앱에서 사용할 수 <br>
있는 동작들이 나온다. 나중에 하나씩 다뤄보도록 하고 이번 포스팅에서는 Pan Gesture Reconigzer를<br>
사용해보자.

## UIPanGestureReconizer

언제나 그렇듯, 개발자의 좋은 자세로 여겨지는 것이 항상 공식 개발 문서를 참고하는 것이다. 공식 문서를 먼저 살펴보면 

***A continuous gesture recognizer that interprets panning gestures.***

패닝 제스쳐를 해석하는 연속적인 제스처 인식기(직역하면 그렇다) 라고 번역이 되는데 여기서 말하는 패닝 동작은<br>
터치 후 드래그 동작을 뜻한다. <br>
(사실 명확하게 뜻이 안나와있어서 여러 다른 분야의 포스팅을 찾아 봤는데 카메라 촬영 기법에도 나오고 포토샵에도<br>
비슷한 용어들 나온다. 포토샵에서는 클릭 후 드래그해서 객체를 움직이는 액션을 뜻한다.)
 
어떤 앱에서 스티커를 이용해서 사진을 꾸밀 때 스티커를 터치해서 움직인 다음 원하는 위치에 배치하는 동작?<br>
으로 생각하면 좋을 것 같고, 그냥 뭔가를 움직이지 않더라도 직선 모션은 다 감지한다.

그럼, 이 제스쳐가 코드에서 어떻게 작동하는지 한번 알아보자.

## EX1 - 단순 뷰 이동!
화면에 떠있는 이미지 뷰를 이동시키는 간단한 코드 예제를 준비해 보았다. 당연히 이미지 뷰는 화면 외부를<br>
튀어나가면 안되는 것을 전제로 만들었다.

일단 스토리보드를 연 다음, 중간에 이미지 뷰를 하나 만들어서 원하는 배경색을 넣든 이미지를 넣어서 배치를<br> 해보자.
(공돌이들에게 디자인은 너무나 어려운 영역인것...)

![Kind of Gesture in UIKit](/post_img/20221026/UIPanGesture_1.png)

그리고 반드시!! 우측 인스펙터 창에서 "User Interaction Enable" 항목을 반드시 체크해야 한다!

그 다음, 해당 뷰컨트롤러에 연결되어 있는 ViewContoller.swift에 가서 스토리보드에 추가해 준 이미지뷰 객체를<br>
선언한 다음, viewDidLoad 함수에 다음 코드를 넣어보자.(아래 코드 참조)

```swift
import UIKit

class ViewController1: UIViewController {

    //추가한 이미지 뷰 객체 선언. 당연히 스토리보드와 연결되어 있어야 함.
    @IBOutlet weak var imageView: UIImageView!
    
    //UIScreen.main.bounds -> 현재 기기의 외곽 사이즈를 CGRect로 반환해줌.
    let bounds: CGRect = UIScreen.main.bounds
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        //난 공돌이이기 떄문에 대강 SF Symbol로 이미지 뷰에 동그라미를 넣어보았다.
        imageView.image = UIImage(systemName: "circle.fill")
        
        //UIView를 상속하는 객체에 제스쳐 전달. #selector 로 콜백함수 전달
        let gesture = UIPanGestureRecognizer(target: self, action: #selector(dragObject))
        
        //제스쳐 등록 -> addGestureRecognizer 메서드는 UIView의 메서드이기 때문에 UIView를 상속받는 모든
        //객체에서 사용 가능하다!
        imageView.addGestureRecognizer(gesture)
    }
    
    //제스쳐 실행 시 콜백 함수 -> 위 이미지 뷰에서 등록한 제스쳐가 실행될 때 마다 dragObject를 실행시키며, 
    //그 매개변수로 UIPangGestureRecognizer 타입의 sender를 보낸다. 
    @objc func dragObject(_ sender: UIPanGestureRecognizer) {
        
        let location = sender.location(in: view) //등록한 이미지 뷰의 현재 위치
        let draggedView = sender.view! //현재 드래그 하고자 하는 뷰. 즉 이미지 뷰를 뜻한다.
        
        if (location.x < imageView.frame.width/2) { 
            draggedView.center.x = imageView.frame.width
            //현재 사람 손의 위치가 스마트폰 좌측에서 이미지 뷰 폭의 1/2구간일 때, 
            //스마트 폰의 좌측으로 튀어나가지 않도록!

        } else if (location.x > bounds.maxX - imageView.frame.width/2) {
            draggedView.center.x = bounds.maxX - imageView.frame.width/2
            //현재 사람 손의 위치가 스마트폰 우측에서 이미지 뷰 폭의 1/2구간일 떄,
            //스마트 폰의 우측으로 튀어나가지 않도록! (bounds.maxX!)

        } else {
            draggedView.center = location
            //그 외에는 움직이는 뷰의 센터는 현재 손가락의 위치를 그대로 따라간다!
        }
    }
}
```
![EX2](/post_img/20221026/EX1.gif)

## EX2 - 뷰 방향 인식!
이번에는 드래그 하는 방향을 인식하도록 만들어 보았다. 위 예제처럼 똑같이 동작하는 뷰를 만들고,<br>
드래그 방향을 라벨에 띄우도록 만들어 보자.

위 코드와 비슷한데, 방향을 문자열로 띄우도록 스토리보드에 라벨을 하나 추가하였고,<br>
dragObject 콜백 함수에 sender.velocity라는 메서드가 하나 더 생겼다. <br>
이는, 드래그 방향에 따라 속도를 CGFloat 으로 반환해 준다.

!! 속도니까 당연히 부호도 있다. +,-로 반환하기 떄문에 어느 방향으로 드래그가 되고있는 지
알 수 있다.
참고로, 기기의 좌측 최상단이 0,0이고, 이를 기준으로 우측으로 가면 +X 방향, <br>
아래로 가면 +Y 방향이다. (다 아실거라 믿습니다!)

```swift
import UIKit

class ViewController2: UIViewController {

    @IBOutlet weak var labelDir: UILabel!
    @IBOutlet weak var controlDirView: UIImageView!
    
    let bounds: CGRect = UIScreen.main.bounds
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        //방향을 보여줄 라벨!
        labelDir.text = "haha"
        
        //나머지는 똑같다.
        let gesture = UIPanGestureRecognizer(target: self, action: #selector(dragObject))
        
        //제스쳐 등록
        controlDirView.addGestureRecognizer(gesture)
    }
    
    //제스쳐 실행 시 콜백 함수
    @objc func dragObject(_ sender: UIPanGestureRecognizer) {
        
        let location = sender.location(in: view)
        let draggedView = sender.view!
        
        //x방향의 속도를 절대값 처리하여 y속도 절대값 보다 클 때, 분기
        if abs(sender.velocity(in: self.view).x) > abs(sender.velocity(in: self.view).y) {
            //x방향 속도가 음수 일 때, (좌측 방향 드래그)
            if (sender.velocity(in: self.view).x < 0) {
                labelDir.text = "좌"
            } else { //그 외
                labelDir.text = "우"
            }
            //y방향의 속도를 절대값 처리하여 x속도 절대값 보다 클 때, 분기
        } else if abs(sender.velocity(in: self.view).x) < abs(sender.velocity(in: self.view).y) {
            //y방향 속도가 양수 일 때, (아래 방향 드래그)
            if (sender.velocity(in: self.view).y > 0) {
                labelDir.text = "하"
            } else { //그 외
                labelDir.text = "상"
            }
        }
        
        if (location.x < controlDirView.frame.width/2) {
            draggedView.center.x = controlDirView.frame.width/2

        } else if (location.x > bounds.maxX - controlDirView.frame.width/2) {
            draggedView.center.x = bounds.maxX - controlDirView.frame.width/2

        } else {
            draggedView.center = location
        }
    }
}
```
![EX2](/post_img/20221026/EX2.gif)

이렇게 하면 뷰가 이동하는 방향을 상/하/좌/우로 나누어서 라벨에 표시가 된다. <br>
(콜백함수는 뷰를 드래그하여 이동시킬 때 마다 실행되기 때문에 라벨에 표시도 계속해서 업데이트 시킨다!)

그리고, 2번 예제에서 뷰가 조금 끊겨 보이는 현상이 있는데, 뭔가 다른 셋팅이 필요한 것 같다. <br>
(매끄럽게 처리하는 방법을 계속 찾아보겠다.)

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
회신 후, 최대한 빠른 시간 내에 회신드릴게요! 감사합니다.