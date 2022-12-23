---
title: iOS에서 쓰는 Swift 클로저! 톺아보기 1편
date: 2022-12-23 15:23:00 +0900
categories: [Swift, Grammer]
tags: [swift, grammer]
author: WalterCho
---

먼저 Swift는 굉장히 현대적인 프로그래밍 언어입니다.<br>
그래서 클로저, 타입추론, 옵셔널등 현대 프로그래밍 언어의 주요기능들을 대부분 채택했는데요.<br>
이 중에서 **클로저**에 대해 알아볼까 합니다.

클로저는 다른 언어에서 람다식, 고차함수등으로 불리기도 하는데, 개념도, 사용법도 거~의 흡사합니다.<br>
특장점도 비슷하죠.
- 클로저는 코드를 더 간결하게 만들 수 있어요.
- 클로저는 함수형 프로그래밍이라서 외부 요인으로 인한 오류를 최소화할 수 있어요.
- 이로인해 클로저는 병렬처리에 유용합니다.

클로저의 기본 형태는 다음과 같습니다.
```swift
{ (parameters) -> return type in
    //TODO
}
```

Swift에서 클로저는 다른 말로 `익명함수`라고도 합니다. 하지만 함수라는 사실은 변함이 없어요.<br>
그래서 함수로써 가져야할 파라미터가 있고, 리턴값이 있습니다.<br>
그런데, 어딘가 iOS앱을 만들면서 써왔던 filter, map등의 함수와 비슷하지 않나요?<br>
네, 맞습니다. 이것들이 바로 클로저에요!

```swift
let scores = [30, 80, 10, 50, 100]
let under50 = scores.filter { score -> Bool in
    //scores라는 배열의 각 요소를 score라는 이름의 파라미터 이름으로 정의,
    //Filter의 Return type은 Bool
    return score < 50
}
//under50 : [30, 10]
```
> parameters는 생략할 수 있어요. <br>
> 생략된 parameter는 $0, $1, $2... 등으로 나타낼 수 있어요.
{: .prompt-info }

그래서 이렇게도 축약할 수 있습니다.
```swift
let scores = [30, 80, 10, 50, 100]
let under50 = scores.filter { return $0 < 50 }
//under50 : [30, 10]
```

## 일급객체
Swift는 클로저는 일급객체로 다뤄지는 데요,<br>
이것이 무슨 말이냐, ***함수를 인자로 보내거나, 함수를 전달하거나, 함수로 반환할 수 있다***는 것입니다.

- 함수를 변수나 상수에 대입해서 사용할 수 있어요.

```swift
let printName = { (name: String) in
    "안녕하세요, \(name)님"
}

let greeting: String = printName("Loperz")
print(greeting)
//안녕하세요, Loperz님
```

> return type과 retrun키워드도 생략할 수 있어요.<br>
> 클로저의 parameter는 오직 parameter Name으로만 사용됩니다.
{: .prompt-info }

- 클로저를 함수의 인자로 사용할 수도 있습니다.

```swift
let add = { (a:Int, b:Int) -> Int in
    return a + b
}

func sum(n1: Int, n2: Int, function: (Int, Int) -> Int) -> Int {
    return function(n1, n2)
}
sum(n1: 2, n2: 3, function: add) //마지막 인자가 클로저면 생략 가능!
//5
```

> 복잡해보일 수 있는 구문이지만, parameter Name과 인자를 구분하면 눈에 들어올거에요!
{: .prompt-info }

그리고 위 구문처럼 함수의 마지막 인자가 클로저라면 이렇게 생략할 수도 있습니다.
```swift
et add = { (a:Int, b:Int) -> Int in
    return a + b
}

func sum(n1: Int, n2: Int, function: (Int, Int) -> Int) -> Int {
    return function(n1, n2)
}

//마지막 인자인 클로저를 생략하고 바로 { } 로 표현할 수 있어요
//이를 후행 클로자라고 합니다!
sum(n1: 2, n2: 3) {
    $0 + $1
}
//5
```

- 클로저를 반환 타입으로 하는 함수를 만들 수 있어요.

```swift
func greeting2(msg: String) -> (String, String) -> String {
    return { greeting, name in
        return name + "님, " + greeting + ". " + msg
    }
}

let closure = greeting2(msg: "화이팅입니다!")
closure("안녕하세요", "Loperz")
```

> greeting2(msg: String) 이 (String, String) -> String 형태의 클로저를 반환합니다.
{: .prompt-info}

마지막 클로저를 반환하는 구문은 상당히 복잡하죠?<br>
하지만 하나씩 살펴보면 충분히 이해하실 수 있을 거에요!

먼저 `greeting2(msg: String) -> `여기까지 보면 일반적인 함수 입니다.<br>
그리고 반환값이 바로 `(String, String) -> String` 형태의 클로저인 것이죠.

때문에 클로저를 return하는 것을 알 수 있어요.
```swift
{ greeting, name in
    return name + "님, " + greeting + ". " + msg
}
```

따로 떼어놓고 보니 딱 클로저의 기본 형태죠?<br>
`greeting2(msg: )`는 이 클로저를 return하게 됩니다.

<hr>

클로저의 개념과 기본 형태, 그리고 약간의 경량 문법도 같이 살펴봤는데요,<br>
현대 프로그래밍 언어인 만큼 코드는 간결해지지만 조금 주의깊게 봐야하는 부분이었습니다!<br>
하지만 하나씩 분해해보니 크게 어렵게 느껴지진 않으셨을 것 같아요.

클로저만 잘 익혀두시면 앞으로 공부하게 될 RxSwift나 SwiftUi에 큰 도움이 됩니다!<br>
다음엔 클로저를 더 쉽고, 간략하게 쓸 수 있는 방법을 포스팅해볼게요, 그게 언제가 될지는 모르겠지만요?ㅎㅎ


## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.