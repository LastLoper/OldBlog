---
layout: post
menubar: gestures_menu
date: 2022-11-05 00:20:00 +0900
title: iOS, UIKit 제스쳐 알아보기 2편 - UISwipeGesture
subtitle:  그러니까...
categories: property-observer
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

iOS, UIKit 제스쳐 알아보기 1편 - UIPanGesture
<http://loperz.com/posts/UIPanGesture/>

이번 포스팅에서는 Swipe 제스쳐에 대해서 알아보도록 하겠다.

## UISwipeGestureReconizer

모든 포스팅에서 강조하듯, 개발자 문서를 한번 보고 넘어가보자! (진짜 웬만한 해답은 제공되는 개발자 문서에 다있는 듯 하다!)

***A discrete gesture recognizer that interprets swiping gestures in one or more directions.***

직역하면, 스와이프 제스처를 하나 이상의 방향으로 해석하는 개별 제스처 인식기 라고 하는데, 말그래도 스와이프 제스쳐와 그 방향에 따라서
콜백을 호출하기 위한 클레스로 이해하면 될 것 같다.

설명 상에서 지난 시간에 다뤘던 Pan 제스쳐와 다른 워딩을 찾아보면

1. Continuous VS Discrete<br>
2. direction

정도가 있는 것 같은데, 설명만 봤을 떄는 스와이프 제스쳐는 연속성을 띄지 않는 제스쳐를 나타내는 것 같기도 하고
제스쳐의 방향이 정형화 된 느낌을 준다. (둘의 차이는 다음 포스팅에서 다룰 예정이다!)

백문불여일견이라고 설명보다는 눈으로 보는게 낫다! 코드로 어떻게 작동하는지 살펴 보자!

## EX1 - 기본 사용법(이라 읽고 삽질 후기)

일단, UIPanGesture 를 한번 사용해봤기 때문에, 모든 제스쳐를 다 할 수 있을것만 같았다.
호기롭게 중간에 이미지 뷰를 하나 넣고, 이 이미지 뷰를 스와이프 하는 방향으로 콘솔을 띄워보고자 했다.

기본적으로 UIPanGesture와 사용방법이 매우 매우 매우 흡사하다. 근데, UISwipeGesture 에서는
제스쳐 인스턴스를 선언하고, 반드시....반드시!!!<br>
***스와이프 방향을 꼭 꼭 꼭 설정해주어야 한다!!!*** <br>
(호기롭게 UIPanGesture 처럼 썼다가 계속 안되서 한참 해맸다...)

참고로, 스와이프 방향을 지정해주지 않으면 우측 스와이프 방향이 기본으로 설정된다. <br>
방향을 설정해주지 않고, 상/하/좌/우 열심히 스와이프 해보는데 계속 우측만 떴다....

아래 코드를 보자.

```swift
import Foundation
import UIKit

class ViewController3: UIViewController {

    @IBOutlet weak var imageView: UIImageView!
    @IBOutlet weak var dirLabel: UILabel!
    @IBOutlet weak var returnButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let gesture = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithSingle))
        
        //이거..이거!!! 꼭 등록해줘야한다. 난 위쪽 방향 스와이프를 등록했다!
        gesture.direction = .up
        
        //제스쳐 등록
        imageView.addGestureRecognizer(gesture)
    }
    
    //제스쳐 실행 시 콜백 함수
    @objc func swipeUpActionWithSingle(_ sender: UISwipeGestureRecognizer) {
        
        let swipeDir = sender.direction
        
        switch swipeDir {
        case .up :
            dirLabel.text = "위로!위로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.up.filled")
        case .down :
            dirLabel.text = "아래로!아래로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.down.filled")
        case .right :
            dirLabel.text = "우로!우로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.right.filled")
        case .left :
            dirLabel.text = "좌로!좌로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.left.filled")
            
        default:
            break
        }
    }
    
    //이건 그냥... 반복해서 보여줄려고 원복시키는 버튼 콜백
    @IBAction func toInitialCondition(_ sender: Any) {
        
        dirLabel.text = "방향"
        imageView.image = UIImage(systemName: "circle.grid.cross")
    }
}
```
![EX1](/img/2022-11-05/uiswipegesture_ex1.gif)

저 플레이스테이션 방향키 같은 이미지 뷰를 위로 쓸어올리면, 변하는 뷰를 볼 수 있다.

## EX2 - 응용(4방향)

별로 다른게 없다. 그냥 제스쳐의 인스턴스를 4개 선언하고, 4방향을 각각 등록해 준 다음 
콜백은 기존에 있던 함수를 써보자.

```swift

import Foundation
import UIKit

class ViewController3: UIViewController {

    @IBOutlet weak var imageView: UIImageView!
    @IBOutlet weak var dirLabel: UILabel!
    @IBOutlet weak var returnButton: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        let gestureUp = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithSingle))
        gestureUp.direction = .up
        
        let gestureDown = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithSingle))
        gestureDown.direction = .down
        
        let gestureLeft = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithSingle))
        gestureLeft.direction = .left
        
        let gestureRight = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithSingle))
        gestureRight.direction = .right

        
        //제스쳐 등록
        imageView.addGestureRecognizer(gestureUp)
        imageView.addGestureRecognizer(gestureDown)
        imageView.addGestureRecognizer(gestureLeft)
        imageView.addGestureRecognizer(gestureRight)
    }
    
    //제스쳐 실행 시 콜백 함수
    @objc func swipeUpActionWithSingle(_ sender: UISwipeGestureRecognizer) {
        
        let swipeDir = sender.direction
        
        switch swipeDir {
        case .up :
            dirLabel.text = "위로!위로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.up.filled")
        case .down :
            dirLabel.text = "아래로!아래로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.down.filled")
        case .right :
            dirLabel.text = "우로!우로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.right.filled")
        case .left :
            dirLabel.text = "좌로!좌로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.left.filled")
            
        default:
            break
        }
    }
    
    @IBAction func toInitialCondition(_ sender: Any) {
        
        dirLabel.text = "방향"
        imageView.image = UIImage(systemName: "circle.grid.cross")
    }
}


```

![EX2](/img/2022-11-05/uiswipegesture_ex2.gif)

포인터가 안보이지만 열심히 움직이고 있다......

## EX3 - 응용2(두손가락 모드)

이번에는 두 손가락을 사용해 보자. "numberOfTouchesRequired" 프로퍼티에다가
터치할 손가락 개수를 숫자로 넣어주면 된다.(해당 프로퍼티는 정수형 setter 프로퍼티다!)

그리고, 원래 있던 코드에다가 한손가락 모드인지 두손가락 모드인지 띄우는 라벨까지 추가해서
한번 보도록 하자.

비교를 위해 한손가락 모드일 떄의 제스쳐 인스턴스와, 두 손가락일 때의 제스쳐 인스턴스 전부 선언하고
똑같이 이미지 뷰에 제스처 등록을 해준다. <br>
그리고, 두 손가락일 때의 제스쳐의 "numberOfTouchesRequired" 프로퍼티에 손가락 2개를 선언해주었다.

아래 코드를 보자!

```swift
import Foundation
import UIKit

class ViewController3: UIViewController {

    @IBOutlet weak var imageView: UIImageView!
    @IBOutlet weak var dirLabel: UILabel!
    @IBOutlet weak var returnButton: UIButton!
    @IBOutlet weak var numberOfFingerLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        let gestureUp = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithOneFinger))
        gestureUp.direction = .up
        
        let gestureDown = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithOneFinger))
        gestureDown.direction = .down
        
        let gestureLeft = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithOneFinger))
        gestureLeft.direction = .left
        
        let gestureRight = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithOneFinger))
        gestureRight.direction = .right

        
        //제스쳐 등록
        imageView.addGestureRecognizer(gestureUp)
        imageView.addGestureRecognizer(gestureDown)
        imageView.addGestureRecognizer(gestureLeft)
        imageView.addGestureRecognizer(gestureRight)
        
        let gestureUpTwoFinger = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithTwoFinger))
        gestureUpTwoFinger.direction = .up
        gestureUpTwoFinger.numberOfTouchesRequired = 2 //손가락 개수!
        
        let gestureDownTwoFinger = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithTwoFinger))
        gestureDownTwoFinger.direction = .down
        gestureDownTwoFinger.numberOfTouchesRequired = 2 //손가락 개수!
        
        let gestureLeftTwoFinger = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithTwoFinger))
        gestureLeftTwoFinger.direction = .left
        gestureLeftTwoFinger.numberOfTouchesRequired = 2 //손가락 개수!
         
        let gestureRightTwoFinger = UISwipeGestureRecognizer(target: self, action: #selector(swipeUpActionWithTwoFinger))
        gestureRightTwoFinger.direction = .right
        gestureRightTwoFinger.numberOfTouchesRequired = 2 //손가락 개수!

        
        //제스쳐 등록
        imageView.addGestureRecognizer(gestureUp)
        imageView.addGestureRecognizer(gestureDown)
        imageView.addGestureRecognizer(gestureLeft)
        imageView.addGestureRecognizer(gestureRight)
        imageView.addGestureRecognizer(gestureUpTwoFinger)
        imageView.addGestureRecognizer(gestureDownTwoFinger)
        imageView.addGestureRecognizer(gestureLeftTwoFinger)
        imageView.addGestureRecognizer(gestureRightTwoFinger)
    }
    
    //손가락 한개 스와이프 콜백
    @objc func swipeUpActionWithOneFinger(_ sender: UISwipeGestureRecognizer) {
        
        let swipeDir = sender.direction
        
        numberOfFingerLabel.text = "손가락 한개 감지"
        
        switch swipeDir {
        case .up :
            dirLabel.text = "위로!위로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.up.filled")
        case .down :
            dirLabel.text = "아래로!아래로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.down.filled")
        case .right :
            dirLabel.text = "우로!우로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.right.filled")
        case .left :
            dirLabel.text = "좌로!좌로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.left.filled")
            
        default:
            break
        }
    }
    
    //손가락 두개 스와이프 콜백
    @objc func swipeUpActionWithTwoFinger(_ sender: UISwipeGestureRecognizer) {
        
        let swipeDir = sender.direction
        
        numberOfFingerLabel.text = "손가락 두개 감지"
        
        switch swipeDir {
        case .up :
            dirLabel.text = "위로!위로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.up.filled")
        case .down :
            dirLabel.text = "아래로!아래로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.down.filled")
        case .right :
            dirLabel.text = "우로!우로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.right.filled")
        case .left :
            dirLabel.text = "좌로!좌로!"
            imageView.image = UIImage(systemName: "circle.grid.cross.left.filled")
            
        default:
            break
        }
    }
    
    @IBAction func toInitialCondition(_ sender: Any) {
        
        dirLabel.text = "방향"
        imageView.image = UIImage(systemName: "circle.grid.cross")
        numberOfFingerLabel.text = "손가락 몇개?"
    }
}

```

![EX2](/img/2022-11-05/uiswipegesture_ex3.gif)

두개일 떈 마우스 포인터가 보이던데... 시뮬레이터 레코딩에서 없는채로 녹화를 해버리네...
어쩄든 열시미 움직이고 있다!