---
layout: page
menubar: control_flow_menu
title: Switch
subtitle: 값 일치 여부를 판단하는 조건문
show_sidebar: false
toc: false
hero_image: /img/control-flow/switch-hero.png
hero_height: is-medium
hero_darken: true
---

`if`문처럼 true 또는 false를 결과로 반환하는 것은 동일하다.

차이가 있다면 `if`문은 조건이 늘어날수록 코드가 길어지고 가독성이 많이 떨어진다.

반면, Switch는 각 조건을 case로 나열함으로써 조금 더 간결해진다는 장점이 있다.

<br/>

이번에도 사과를 예로 들어보자,

내가 🍎와 🍏를 들고 있을때, 

손님이 🍎를 산다면 개당 2,000원에 팔 것이고, 🍏를 산다면 개당 1,000원에 팔 것이다.

<br/>

`switch`문이 바로 이것이다. 각 질문에 해당하는 구문을 실행하는 것.

`if`문이 "그건 사과가 맞지?"하고 물어보는 것이라면,

 `switch`문은 "네가 들고 있는게 뭐야? 혹시 사과는 있어?"하고 물어보는 것이다.

<br/>

각 case는 반드시 하나의 실행 가능한 구문이 또는 break, fallthrough 키워드가 있어야 한다.

이때, 실행 구문이 있다면 `break`를 쓰지 않아도 된다. 구문을 처리한 후 자동으로 종료하기 때문.

![When no any contents error](/img/2022-09-27/when_no_any_contents_error.png)
_실행구문이 없을 때 break, fallthrough까지 없다면 발생하는 오류_

<br/>

enum class와 같이 정해져 있는 case가 있다면 default문은 필요없다.

그것이 아니라면, 이런 오류를 볼 수 있다.

![When no default error](/img/2022-09-27/when_no_default_error.png)
_default를 써주지 않았을 때 발생하는 오류_

<br/>

## 사용법

```swift
let 🍎 = "사과"
switch 음식 {
    case "사과":
        print("개당 2,000원에 팔게!")
    case "풋사과":
        print("개당 1,000원에 팔게!")
    default:
        print("다른 과일은 없어")
}
//개당 2,000원에 팔게!
```

,(쉼표)를 사용해서 case를 한 줄로 작성할 수 있다.

```swift
let 🍎 = "사과"
switch 🍎 {
case "사과", "홍로", "아오리", "부사":    
    print("사과입니다.")
case "단감", "홍시", "곶감":
    print("감입니다!")
default:
    print("잘 모르겠어요..")
}

//사과입니다.
```

case에 범위를 줄 수도 있다. Range 또는 ClosedRange 모두 사용 가능하다.<br>

```swift
let money = 2000
switch money {
case 1000..<2000:
    print("🍏가 개당 천원인데.. 2개 드릴게요!")
case 2000..<3000:
    print("🍏 2개 아니면.. 🍎 1개 사실 수 있으시겠네요.")
default:
    print("원하는 게 뭐에요!?")
}

//🍏 2개 아니면.. 🍎 1개 사실 수 있으시겠네요.
``` 

<br/>

### Fallthrough 키워드

`switch`문은 실행할 구문이 있을 때, 다음 case를 실행하지 않고 자동 종료한다.

하지만 가끔은, 아래 case를 실행할 필요가 있는 경우도 있다. 이때 쓸 수 있는 것이 `fallthrough` 키워드다.

위 코드에서는 1000..<2000 을 써서 case의 조건이 중복되는 것을 막았지만, 만약 아래와 같이 1000...2000 를 쓴다면 조건이 중복될 수도 있다.

이런 상황에서 최고의 방법은 물론 조건을 분명하게 하는 것이겠지만, 아닌 경우도 있으니까..

그렇다고 조건을 무시할 수는 없으니 `fallthrough` 키워드를 써주면 된다.

```swift
let money = 2000
switch money {
case 1000...2000:
    print("🍏가 개당 천원인데.. 2개 드릴게요!")
    fallthrough
case 2000..<3000:
    print("🍏 2개 아니면.. 🍎 1개 사실 수 있으시겠네요.")
default:
    print("원하는 게 뭐에요!?")
}

//🍏가 개당 천원인데.. 2개 드릴게요!
//🍏 2개 아니면.. 🍎 1개 사실 수 있으시겠네요.
```