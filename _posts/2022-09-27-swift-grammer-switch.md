---
title: Swift 값의 일치 여부를 판단하는 Switch문 이해하기
date: 2022-09-27 21:47:00 +0900
categories: [Swift, Grammer]
tags: [swift, grammer, 조건문]
author: WalterCho
---

Swift는 참과 거짓을 판별하는 조건문 혹은 분기문을 2가지 제공한다.<br>
`If - else if - else문(이하 if문)`과 `Switch - case문(이하 switch문)`이다.

If - else if - else문은 아래 링크에서 확인할 수 있다.<br>
<https://lastloper.github.io/posts/swift-basic-if/>

## 개념
if문이 문에 맞는 키를 찾아 문을 여는 방식이라면,<br>
Switch문은 마치 다양한 보기중에 하나를 선택하는 객관식 문제와 비슷하다.
![Switch Concept](/post_img/20220927/switch_concept.png)

주어진 보기(case문)중에서 일치하는 case의 구문을 실행하는 것이다. 사용방법은 간단하다.

## 사용법
### 기본
기본적인 사용방법이다. 다른 언어들과 크게 다르지 않은 모양인데, Swift는 기본적으로 내용이 있을때는 break를 써주지 않아도 된다.

```swift
var color = "Purple"
switch color {
case "Green", "Red", "Blue":
    //각각 나열할 수 있지만 ,(쉼표)를 사용해서 하나로 처리할 수도 있다.
    print("빛의 삼원색에 포함됩니다.")
default:
    print("빛의 삼원색에 포함되지 않습니다.")
}

//빛의 삼원색에 포함되지 않습니다.
```

### 범위연산자
범위연산자를 case로 줄 수 있다. Range 또는 ClosedRange 모두 사용 가능하다.<br>

```swift
var myMoney = 1000...5000
switch myMoney {
case 100...999:
    print("현재 물가로 육개장 사발면도 못사요!")
case 1000...5000:
    print("다이소 물건을 살 수 있어요!")
case 5000...15000:
    print("한 끼 식사를 할 수 있어요!")
case 15000...50000:
    print("영화는 못보지만 둘이 술 한잔 할 수 있어요!")
default:
    print("하고 싶은 걸 다 할 수 있어요~!")
}

//다이소 물건을 살 수 있어요!
``` 

### 튜플
튜플은 잘 알다시피 다양한 타입의 값을 가지는 변수 또는 상수를 말한다.<br>
아래와 같이 ***team***으로 정의된 상수가 바로 튜플이다.
```swift
let team:(String, Int) = ("Loperz", 2)
let (teamName, countOfMember) = team
print("팀명 : \(teamName), 멤버수 : \(countOfMember)명")

//팀명 : Loperz, 멤버수 : 2명
```

이제 이 튜플을 조건으로 Switch문에서 값을 매칭할 수 있으며,<br>
case문내에서만 사용할 수 있는 상수를 만들어 값을 바인딩해서 사용할 수 있다.

```swift
switch team {
case (let name, 0..<1000):
    print("\(name)은 새내기 개발팀입니다.")
case ("Loperz", 100000..<60000000000):
    print("Loperz의 멤버는 전세계 개발자들과 함께합니다.")
default:
    print("Loperz는 열심히 성장하고 있는 중입니다.")
    break
}

//Loperz은 새내기 개발팀입니다.
```

## 알아두면 쓸데있는 개발팁
1. Switch문의 각 case에는 반드시 break, fallthrough 또는 내용이 있어야 한다.
![When no any contents error](/post_img/20220927/when_no_any_contents_error.png)

2. 모든 case를 작성하지 않는다면 default를 작성해 줘야한다.
![When no default error](/post_img/20220927/when_no_default_error.png)

1. Switch문 case를 처리한 후 자동으로 빠져나오지만 아래 case까지 실행하고 싶을 때, `fallthrough`를 작성해주면 된다.<br>
위 예제에서 범위를 case로 했을 때 각 case의 조건이 겹치는 것을 발견할 수 있다. 이때 fallthrough 명령어를 입력해주면 해당하는 모든 case구문을 처리한다.

```swift
case 1000...5000:
    print("다이소 물건을 살 수 있어요!")
    fallthrough     //멈추지 않고 조건과 일치하는 아래 구문까지 실행하도록 한다.
case 5000...15000:
    print("한 끼 식사를 할 수 있어요!")
    .
    .
    .
}

//다이소 물건을 살 수 있어요!
//한 끼 식사를 할 수 있어요!
```

## 퀴즈
이제 막 Swift를 시작한 프로그래밍 초보인 분들을 위한 퀴즈입니다. 정답을 직접 코딩해서 저희 로퍼즈 메일로 보내주세요!<br>
메일 주소는 왼쪽 사이드 메뉴의 하단 메일 버튼을 이용해주세요, 바로 메일을 보낼 수 있답니다.

```swift
//a와 b는 아래와 같이 돈을 가지고 있습니다.
let money:(Int, Int) = (3000, 1000)
let (aMoney, bMoney) = money

//이때 switch문을 이용해서 둘이 같은 금액을 가지고 있는지 아닌지 출력해보세요.
//같은 금액이라면 "a와 b가 가진 돈은 같습니다."
//다른 금액이라면 "a는 3000원, b는 1000원의 돈을 가지고 있습니다."

//hint. Switch문은 where명령어를 사용해서 값을 조금 더 정밀하게 비교할 수 있다.
```

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.