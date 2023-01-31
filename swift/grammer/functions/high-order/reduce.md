---
layout: page
menubar: functions_menu
title: Reduce
subtitle: Array 등에서 요소들을 결합해 반환하는 고차함수
show_sidebar: false
toc: false
hero_image: /img/functions/reduce-hero.png
hero_height: is-medium
hero_darken: true
---

***Returns the result of combining the elements of the sequence using the given closure.***

***주어진 조건을 이용해서 기존의 값들을 결합한 새로운 컨테이너를 반환합니다.***

<br/>

`map`과 마찬가지로 단어의 뜻과 기능이 서로 다른거 같아 잘 사용하지 않았던 고차함수다.

그런데 Array 등의 요소들을 모아 합치거나 빼거나 해서 의미있는 결과 하나로 줄인다고 생각하니 그때부터 꽤 유용하게 사용하고 있다.

<br/>

`filter`, `map`과는 달리 필수적으로 전달해야 하는 파라미터가 있다.

- initialResult: Result
  - 초기값, 클로저가 실행될 때 nextPartialResult로 전달한다.

- nextPartialResult: (Result, Self.Element) throws -> Result
  - 연산된 결과가 누적되는 파라미터, 클로저이므로 결과를 누적했다가 반환한다.

<br/>

## 사용법

```swift
let numbers = [1, 2, 3, 4]
let numberSum = numbers.reduce(0, { x, y in
    x + y
})
// numberSum == 10
```

initialResult 파라미터의 값은 0, 

즉 초기값이 0이며, 이 값에 x + y 한 값을 더해 Return한다는 의미이다.

<br/>

이렇게 축약해서 표현할 수도 있다.

```swift
//클로저의 인자를 제거하고 $0, $1로 대체 가능
let numberSum2 = numbers.reduce(0) {
    $0 + $1
}

//nextPartialResult 파라미터를 연산자로 대체 가능
let numberSum3 = numbers.reduce(0, +)
```

<br/>

## 출처
- [Apple Document For Reduce](https://developer.apple.com/documentation/swift/array/reduce(_:_:))