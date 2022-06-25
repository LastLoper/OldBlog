---
title: Segue로 화면 전환 하는 방법
date: 2022-06-19 17:00:00 +0900
categories: [ios, guide]
tags: [ios, guide]     # TAG names should always be lowercase
author: Walter
---

iOS앱은 화면과 화면을, 컨트롤러와 컨트롤러를 전환하는 방법이 꽤 쉽다. XCode IDE가 병맛인 것만 빼면 최대 몇 십초이내에 화면을 전환할 수 있다.<br>
이 페이지는 가장 기본적인 Segue를 이용한 화면전환 방법이다.

## 스토리보드에서는 이렇게
### 컨트롤러 배치
![storyboard](/post_img/0619/move_with_segue.png){: width="972" height="972" style="max-width: 95%" .shadow }
_ViewController를 2개 배치_

두 개의 ViewController를 배치한 후, Segue를 연결하는 방법은 2가지 있다. 특정 뷰에 직접 연결하거나 컨트롤러끼리 연결하는 방법이다. 상황에 따라 골라 쓰면 되겠다.

### Segue 연결, 첫번째 방법
첫 번째 방법은, `Button과 같은 View`와 `ViewController를 연결`하는 것이다. `마우스 오른쪽 버튼을 클릭`해서 메뉴를 호출, 드래그해서 연결한다.

![right click](/post_img/0619/way1_right_click.png){: }
-마우스 오른쪽 버튼을 눌러 연결-

혹은 

`컨트롤(Ctrl) 키를 누른 상태에서 마우스로 드래그`하면 된다.
마우스 오른쪽 버튼을 눌러 Touch Up Inside와 같이 특별한 동작을 요구하지 않는 한 이 방법이 더 간결하다.
> 컨트롤(Ctrl) 키를 이용한 방법은 Button의 IBAction함수를 만들때에도 사용할 수 있다.
{: .prompt-info }
![way2](/post_img/0619/segue_connect_way_1.png){: width="972" height="972" style="max-width: 95%" .shadow }
_컨트롤 키를 누른 상태에서 드래그로 연결_

그리고 손을 떼면 아래와 같은 Manual Segue를 볼 수 있는데, 

![manual segue](/post_img/0619/manual_segue.png){: }
_Manual Segue에서 등장 방식을 선택_

Show 또는 Preset Modally를 선택한다.
> 스마트폰 앱에서는 Show, Present Modally를 사용한다.
{: .prompt-info }

![complete](/post_img/0619/complete.png){: }
_Segue가 연결된 것을 확인_

이제 앱을 실행해보면 어떠한 코드도 작성하지 않았지만 해당 버튼을 눌렀을 때 화면이 전환되는 것을 볼 수 있다.

- 특정 View에 직접 Segue를 연결하는 이 방법은 별도의 @IBAction함수가 필요없다. Storyboard에서 각 Controller를 연결해주는 것만으로도 화면을 전환할 수 있기 때문이다.
- 화면 전환 기능을 하는 버튼이 2개 미만일 때 사용하는 것을 추천한다. 스토리보드가 수많은 Segue로 더러워지는 것이 싫다면 말이다.

### Segue 연결, 두번째 방법
두 번째 방법은, `ViewController`와 `ViewController를 연결`하는 것이다. 첫번째 방법이 특정 View에만 Segue를 연결해서 해당 View로만 화면을 전환하도록 했다면, 이 방법으로는 약간의 코드가 필요하다. 하지만 모든 View에 화면을 전환할 수 있는 기능을 줄 수 있다.

![way1](/post_img/0619/segue_connect_way_2.png){: width="972" height="972" style="max-width: 95%" .shadow }
_ViewController와 ViewController 연결_

3개의 ViewController를 더 추가해서 컨트롤(Ctrl) 키를 누른 상태로 드래그해서 각각의 뷰를 연결했다.
![connected segue](/post_img/0619/connected_segue.png){: }
_ViewController와 ViewController끼리 연결된 모습_

### .swift파일과 연결
이제 각 ViewController와 .swift 파일을 연결해준다.

![create class](/post_img/0619/create_class.png){: }
_각각 클래스를 만들어 연결_

### segue identifier정의
이렇게 연결된 상태에서 각각의 Segue에 Identifier를 준다.

![segue identifier](/post_img/0619/segue_identifier.png){: }
_Segue Identifier 정의_

### IBAction함수 만들기
각 Button의 IBAction함수를 만들어 이벤트를 받는다.

![create ibaction](/post_img/0619/create_ibaction.png){: }
_Button의 IBAction_

3개의 버튼 모두 IBAction함수를 만들었다면 아래와 같은 모습이 되어야 한다.

![complete ibactions](/post_img/0619/complete_ibactions.png){: }
_Inspectors와 IBAction완성_

이 방법에서는 Storyboard에서 Class와 Segue Identifier를 정의 하는 등 조금 더 작업이 필요했다.<br>
이제 약간의 코드만 작성하면 완성이다.

## 코드
화면을 전환하는 코드는 단 한줄이다.
```swift
performSegue(withIdentifier: "Segue Identifier", sender: self)
```

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

이제 앱을 실행해보자. 이제 단 한줄의 Segue로 3개의 버튼 모두 각 ViewController를 띄울 것이다.