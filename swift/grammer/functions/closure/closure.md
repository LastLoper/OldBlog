---
layout: page
menubar: functions_menu
title: Swift의 이름없는 함수, 클로저
subtitle: 함수형 프로그래밍의 기초가 되는 클로저
show_sidebar: false
toc: false
hero_image: /img/functions/closure-hero.png
hero_height: is-medium
hero_darken: true
---

Swift는 현대적인 프로그래밍 언어로써 타입추론, 옵셔널등 현대 프로그래밍 언어의 주요기능들을 대부분 채택했다.

그 중에는 다른 언어에서 람다식, 고차함수등으로 불리는 **클로저, 즉 익명함수**도 있다.

먼저 클로저의 특징을 보면,

- 코드를 더 간결하게 만들 수 있다.
- 함수형 프로그래밍을 따르고 있어 외부 요인으로 발생할 수 있는 오류를 최소화할 수 있다.
- 이로인해 클로저는 병렬처리에 유용하다.

<br/>

## 클로저의 기본 형태

```swift
{ (parameters) -> return type in
    //TODO
}
```

위에서 언급했지만, 클로저는 `익명함수`라고도 한다.

일반적으로 쓰는 함수와 다른 점은 없다. 파라미터도 있고, 리턴값도 있다.

다만 `func addNumbers()`와 같이 이름이 없을 뿐.

<br/>

만약 filter, map 등의 고차함수를 안다면, 맞다. 그 함수들 역시 클로저에 속한다고 보면 된다.

다른 설명 필요 없이 사용법부터 보면 이해가 조금 더 쉽다.

<br/>

## 사용법

```swift
//filter 사용 예제
let scores = [30, 80, 10, 50, 100]
let under50 = scores.filter { score -> Bool in
    return score < 50
}
//under50 : [30, 10]
```

딱 클로저의 기본 형태를 따르고 있다.

여기서 `score -> Bool`와 `return`이라는 키워드는 삭제 할 수 있다.

이때, `score`라는 parameter명은 $0, $1.. 등으로 바꿔서 나타낼 수 있다.

그래서 이렇게 축약 가능하다.

```swift
let scores = [30, 80, 10, 50, 100]
let under50 = scores.filter { $0 < 50 }
print(under50)
//[30, 10]
```

<br/>

## 일급객체

클로저는 일급객체로 다뤄진다.

**함수를 인자로 보내거나, 함수를 전달하거나, 함수로 반환할 수 있는 하나의 객체**는 의미이다.

<br/>

### 함수를 변수나 상수에 대입해서 사용하는 예제다

```swift
let printName = { (name: String) in
    "안녕하세요, \(name)님"
}

let greeting: String = printName("Loperz")
print(greeting)
//안녕하세요, Loperz님
```

위 코드는 아래와 같이 일반 함수로도 작성할 수도 있지만, 일반 함수는 변수나 상수에 전달할 수는 없다.

```swift
func printName(name: String) {
    print("안녕하세요, \(name)님")
}
printName("Loperz")
//안녕하세요, Loperz님
```

하지만 클로저는 함수를 변수나 상수에 전달할 수 있는 것이 일급객체의 한 부분이다.

<br/>

### 클로저를 함수의 인자로 사용하는 예제다.

```swift
let add = { (a:Int, b:Int) -> Int in
    return a + b
}

func sum(n1: Int, n2: Int, function: (Int, Int) -> Int) -> Int {
    return function(n1, n2)
}
sum(n1: 2, n2: 3, function: add)
//5
```

위 예제에서도 볼 수 있듯이 parameter로 함수를 전달한다.

하나씩 구분해서 본다면 특별히 어려운 구문은 아니지만, 뭔가 복잡하고 어려워 보이긴 한다.

이 표현을 아래와 같이 간단하게 나타낼 수도 있다.

Swift에서는 마지막 인자가 클로저인 경우 생략할 수 있기 때문이다.

```swift
//위 코드에서 마지막 인자인 클로저를 생략 가능하다.
...
sum(n1: 2, n2: 3) {
    $0 + $1
}
//5
```

이렇게 나타내는 방법을 **후행 클로저**라고 한다.

<br/>

### 반환 타입을 클로저로 내보낼 수 있다.

```swift
func greeting2(msg: String) -> (String, String) -> String {
    return { greeting, name in
        return name + "님, " + greeting + ". " + msg
    }
}

let closure = greeting2(msg: "화이팅입니다!")
closure("안녕하세요", "Loperz")
```

잘 보면 이해하는데는 큰 문제가 없지만, 클로저의 기본 형태를 그대로 쓰면 조금 복잡해 보인다.

그래서 어려운 것 같기도 하고..

이 예제는 말 그대로 클로저를 Return하는데, return 구문만 떼어놓고 보면 딱 클로저의 기본 형태다.

```swift
{ greeting, name in
    return name + "님, " + greeting + ". " + msg
}
```

<hr>

클로저에 대해 정리했지만, 클로저는 함수형 프로그래밍의 기초가 된다.

이후에 다루게 될 RxSwift 또는 SwiftUi 에서는 대부분 이런 형태로 코드를 작성하는데,

외부 변수로부터 안전하고 재사용성이나 코드 간결성이 더 좋은 것이 강점인 것 같다.