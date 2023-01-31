---
layout: page
menubar: functions_menu
title: Filter
subtitle: Array 등에서 특정 요소들을 걸러내는 고차함수
show_sidebar: false
toc: false
hero_image: /img/functions/filter-hero.png
hero_height: is-medium
hero_darken: true
---

***Returns an array containing, in order, the elements of the sequence that satisfy the given predicate.***

***주어진 조건을 만족하는 값들을 새로운 컨테이너로 반환합니다.***

<br/>

Apple 공식문서에서도 확인할 수 있지만 `Filter`는 단어에서도 알 수 있듯이 걸러내는 역할을 한다.

Array, Dictonary 등의 Collection Type에서 지원하며 조건을 만족하는 값들을 모아 새로운 배열을 만들어 반환한다.

즉, Array Type으로 Return한다는 뜻.

<br/>

## 사용법

`numbers`라는 배열에서 짝수와 홀수를 골라 각각 evenNumbers, oddNumbers라는 새로운 배열을 만들었다.

```swift
let numbers = [10, 1, 23, 45, 62, 73, 88]
let evenNumbers = numbers.filter { number in
    number % 2 == 0
}
print(evenNumbers)
//[10, 62, 88]

let oddNumbers = numbers.filter { number in
    number % 2 != 0
}
print(oddNumbers)
//[1, 23, 45, 73]
```

여기서 추측할 수 있는 것은 클로저 안에서 그 결과가 true에 해당하는 값들만 가져온다는 것이고.

또 하나는 기존 배열인 `numbers`라는 배열은 아무런 영향도 받지 않는다는 것이다.

<br/>

`People` Dictonary에서 나이가 30보다 작은 사람을 골라낼 수 있다.

```swift
let people = ["Walter": 36, "John": 45, "HyoJin": 23, "Taeri": 18]
let under30 = people.filter { person in
    person.value < 30
}
print(under30)
// ["Taeri": 18, "HyoJin": 23]
```

<br/>

이번엔 애플 공식문서에서 제공하는 예시다. `filter`를 축약해서 더 간결한 코드를 보여준다.

cast 요소의 문자열 길이가 5글자보다 작은 이름만을 골라서 새로운 배열로 반환한 것을 알 수 있다.

```swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]
let shortNames = cast.filter { $0.count < 5 }
print(shortNames)
// Prints ["Kim", "Karl"]
```

<br/>

## 출처
- [Apple Document For Filter](https://developer.apple.com/documentation/swift/sequence/filter(_:))