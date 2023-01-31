---
layout: post
menubar: segue_menu
date: 2022-06-19 17:00:00 +0900
title: iOS앱 개발 Storybard에서 Segue로 화면 전환하기
subtitle: GUI로 손쉽게 화면을 전환할 수 있는 Segue
categories: segue
description: GUI로 손쉽게 화면을 전환할 수 있는 Segue
published: true
---

iOS 앱 개발에서 화면전환은 GUI에서 마우스를 잡아 끄는 것 만으로도 가능하다. 이를 Segue 연결이라고도 하는데 특정 뷰를 터치했을 때 화면을 전환할 수 있는 방법과 컨트롤러 자체에서 연결하는 방법 2가지가 있다. 하나씩 정리했다.

<br/>

## 스토리보드

### 컨트롤러 배치

먼저 두 개의 컨트롤러를 Storyboard에 배치한다.

FirstViewController에서 SecondViewController로 화면을 전환할 것이다.

![storyboard](/img/2022-06-19/move_with_segue.png){: width="80%" }

_ViewController를 2개 배치_

<br/>

### 특정 뷰에서 직접 연결

첫 번째로, Button과 같은 View를 터치했을 때 화면을 전환하는 방법이다.

<br/>

마우스 오른쪽 버튼을 클릭해서 메뉴를 호출해서 action을 잡아 끌어 다른 ViewController에 연결한다.

![right click](/img/2022-06-19/way1_right_click.png){: width="80%" }

_View의 메뉴를 호출해서 action을 연결_

이 방법의 장점은 touch Up Inside/Outside 등의 터치 상태에 따른 Action을 직접 선택할 수 있다는 점이다.

<br/>

이를 더 간편히 하는 방법도 있다.

뷰를 클릭, 컨트롤(Ctrl) 키를 누른 상태 마우스로 드래그한다.

Touch Up Inside등과 같은 터치 상태에 따라 특별한 동작이 필요하지 않다면 더 빠르고 간단하다.

![way2](/img/2022-06-19/segue_connect_way_1.png){: width="80%" }

_컨트롤 키를 누른 상태에서 드래그로 연결_

이제 마우스에서 손을 떼면 아래와 같은 Manual Segue를 볼 수 있는데,

SecondViewController가 어떻게 등장하는지, 어떤 애니메이션을 가지고 나타나는지 지정할 수 있다.

![manual segue](/img/2022-06-19/manual_segue.png){: width="80%" }

_Manual Segue에서 등장 방식을 선택_

iOS에서는 Show 또는 Preset Modally를 선택한다.

그 외에는 iPad 등의 다른 기기에서 작동한다.

<br/>

![complete](/img/2022-06-19/complete.png){: width="80%" }

_Segue가 연결된 것을 확인_

ViewContoller 사이에 Segue가 나타나 연결되었다면 이제 연결한 버튼을 터치 했을 때 화면이 전환 된다.

<br/>

### 컨트롤러 자체에서 연결

두 번째는, ViewController자체에서 연결하는 것이다.

말 만들어서는 한번에 잘 와닿지 않는데, 이미지부터 보자.

![way1](/img/2022-06-19/segue_connect_way_2.png){: width="80%" }

_ViewController와 ViewController 연결_

화면에서 화면 전환 버튼이 1~2개 정도만 있다면 뷰에 직접 연결하는 것이 편하다.

하지만 위 이미지처럼 버튼이 여러개일 땐 하나씩 연결하기는 것이 좀 불편할 수 있다.

그래서 여러개의 버튼에 직접 연결하기 보다 하나의 Segue로 관리할 수 있다. 다만 이 방법은 약간의 코드가 필요하다.

![connected segue](/img/2022-06-19/connected_segue.png){: width="80%" }

_ViewController와 ViewController끼리 연결된 모습_

<br/>

### .swift파일과 연결

이제 각 ViewController와 .swift 파일을 연결해준다.

![create class](/img/2022-06-19/create_class.png){: width="80%" }

_각각 클래스 파일을 만들어 연결_

<br/>

### Segue Identifier정의

이렇게 연결된 상태에서 각각의 Segue Identifier를 준다.

![segue identifier](/img/2022-06-19/segue_identifier.png){: width="80%" }

_Segue Identifier 정의_

<br/>

### IBAction함수 만들기

각 Button의 IBAction함수를 만들어 이벤트를 받는다.

![create ibaction](/img/2022-06-19/create_ibaction.png){: width="80%" }

_Button의 IBAction_

<br/>

3개의 버튼 모두 IBAction함수를 만들었다면 아래와 같은 모습이 되어야 한다.

![complete ibactions](/img/2022-06-19/complete_ibactions.png){: width="80%" }

_Inspectors와 IBAction완성_

<br/>

## 코드

버튼이 있는 ViewContoller에서 아래의 코드를 작성하면 된다.

화면을 전환하는 코드는 단 한줄이다.

```swift
performSegue(withIdentifier: "Segue Identifier", sender: self)
```

<br/>

물론, 이는 단순하게 화면만 전환하는 코드일 뿐이고, 데이터를 넘기는 등의 작업은 한 줄보다는 많은 코드를 작성해야 한다.

이제 아까 정의해준 Segue Identifier를 파라미터로 각 Button의 IBAction에 작성한다.

```swift
class ViewController: UIViewController {

    override func viewDidLoad() { ... }

    @IBAction func goThirdView(_ sender: UIButton) {
        performSegue(withIdentifier: "ThirdView", sender: self)
    }
    
    @IBAction func goFourthView(_ sender: UIButton) {
        performSegue(withIdentifier: "FourthView", sender: self)
    }
    
    @IBAction func goFifthView(_ sender: UIButton) {
        performSegue(withIdentifier: "FifthView", sender: self)
    }   
}
```