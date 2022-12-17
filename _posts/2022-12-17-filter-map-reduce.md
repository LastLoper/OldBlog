---
title: Swift 이것만 잘써도 프로! Filter, Map, Reduce에 확실하게 잡기
date: 2022-12-17 15:19:00 +0900
categories: [Swift, Grammer]
tags: [swift, grammer, filter, map, reduce]
author: WalterCho
---

이른바 컬렉션이라 부르는, `Array`와 `Dictionary`는 각 요소를 쉽게 골라내거나(`filter`), 변환하거나(`map`), 합치는(`reduce`) 클로저를 지원하고 있어요! 물론 이 외에도 `forEach`, `compactMap` 등이 있긴 한데, 이  세가지만 잘 써도 만들고자 하는 앱은 충~분히 다 만들 수 있고 가장 대표적이기도 합니다. 무엇보다 적재적소에서 잘 쓰면 데이터를 가공하는 일은 식은 죽 먹기!


## Filter

***Returns an array containing, in order, the elements of the sequence that satisfy the given predicate.***
***주어진 조건을 만족하는 값들을 새로운 컨테이너로 반환합니다.***

애플 개발 문서에도 정의하고 있는데요, 말 그대로 많은 데이터 중에 조건 맞는 데이터를 골라내서 새로운 컨테이너로 Return하는 역할을 합니다.<br>
애플에서 제공하는 예시를 볼까요?
```swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]
let shortNames = cast.filter { $0.count < 5 }
print(shortNames)
// Prints "["Kim", "Karl"]"
```

별다른 설명이 필요없을 정도로 이해가 쉬운 예시인 것 같죠? 각각의 이름 길이가 5글자보다 작은 이름만을 골라서 새로운 배열로 반환한 것을 알 수 있습니다.

## Map

***Returns an array containing the results of mapping the given closure over the sequence’s elements.***
***기존의 값들을 주어진 조건으로 할당(mapping)해서 새로운 결과 컨테이너틀 반환합니다.***

이번엔 각 요소를 이용해서 기존의 값들을 변환, 새로운 형태의 컨테이너로 Return하는 클로저입니다.<br>
한번에 와닿지 않을 수도 있습니다, 하지만! 막상 써보면 그렇게 어렵지 않아요.<br>
애플에서 제공하는 예시를 한번 볼까요?
```swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]
let lowercaseNames = cast.map { $0.lowercased() }
// 'lowercaseNames' == ["vivien", "marlon", "kim", "karl"]

let letterCounts = cast.map { $0.count }
// 'letterCounts' == [6, 6, 3, 4]
```

역시 특별한 설명이 필요없을 정도로 직관적이죠?<br>
첫 번째 예시는 모두 소문자로 변환한 배열을 Return하는 구문입니다.<br>
소문자로 바꾼 것 뿐이니 `lowercaseNames`는 역시 String 타입의 배열입니다.
![using map result1](/post_img/20221217/map_1.png){: }

두 번째 예시는 각 이름의 길이를 반환하는 군요.<br>
그럼 `lotterCounts`의 타입은 어떨까요?
![using map result2](/post_img/20221217/map_2.png){: }

$0.count는 Int형이니 `letterCounts` 배열의 타입이 Int로 바뀐 것이 보이네요!

조금 헷갈릴 수 있는 부분이니 조금만 더 살펴볼까요?<br>
만약 두 번째 예시를 for-in 구문으로 만들었다면 아래와 같이 만들 수 있습니다.
```swift
var letterCounts:[Int] = []
cast.forEach { str in
    let strCount = str.count
    letterCounts.append(strCount)
}
// `letterCounts` == [6, 6, 3, 4]
```

흠 역시 고차함수 map을 이용한게 직관성이 더 높아보이죠?<br>
그런데 컴파일러의 최적화 성능도 고차함수를 이용하는 것이 더 좋다고 합니다!

## Reduce

***Returns the result of combining the elements of the sequence using the given closure.***
***주어진 조건을 이용해서 기존의 값들을 결합한 새로운 컨테이너를 반환합니다.***

마지막으로 배열의 모든 요소들을 더하기, 빼기, 곱하기, 나누기등 특별한 연산으로 결합해서 새로운 컨테이너를 Return하는 클로저입니다.

그런데 filter, map과는 다르게 2가지 파라미터가 필요해요.
- initialResult: Result
  - 초기값, 클로저가 실행될 때 nextPartialResult로 전달합니다.
- nextPartialResult: (Result, Self.Element) throws -> Result
  - 연산된 결과가 누적되는 파라미터 입니다. 클로저이므로 결과를 누적했다가 반환하게 됩니다.

Reduce 예시 입니다.
```swift
let numbers = [1, 2, 3, 4]
let numberSum = numbers.reduce(0, { x, y in
    x + y
})
// numberSum == 10
```

이 코드를 조금 설명하자면, initialResult의 값으로 0을 줬네요.<br>
그리고 x, y는 각 $0과 $1입니다. 즉, initialResult 값을 초기값으로 0을 전달하면서 `numbers`배열의 각 요소를 더하는(x + y) 결과를 Return하는 것입니다.

위 예시는 이렇게도 표현 가능합니다.
```swift
//클로저의 인자를 제거하고 $0, $1로 대체 가능
let numberSum2 = numbers.reduce(0) {
    $0 + $1
}

//nextPartialResult 파라미터를 연산자로 대체 가능
let numberSum3 = numbers.reduce(0, +)
```

## 결론
결과적으로 보면 filter, map, reduce는 for-in문 또는 forEach문으로 대체가 가능한 함수들입니다. 그런데 map에서도 다뤄봤지만 코드가 아주 간결해짐을 알 수 있었습니다. 더군다나 컴파일러의 최적화 성능 또한 더 좋아진다고 하니 안쓸 수가 없겠죠?? Array와 Dictionary를 다룬다면 잊지 마세요~!

이 세가지 고차함수를 알고 적재적소에서 잘 써먹는다면 능히 데이터를 다루는 일에 있어서는 자부심을 가져도 좋을 것 같습니다!

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.