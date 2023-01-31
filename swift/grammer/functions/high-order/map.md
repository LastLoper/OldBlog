---
layout: page
menubar: functions_menu
title: Map
subtitle: Array 등에서 요소의 Type을 변경하는 고차함수
show_sidebar: false
toc: false
hero_image: /img/functions/map-hero.png
hero_height: is-medium
hero_darken: true
---

***Returns an array containing the results of mapping the given closure over the sequence’s elements.***

***기존의 값들을 주어진 조건으로 할당(mapping)해서 새로운 결과 컨테이너틀 반환합니다.***

<br/>

개인적으로 `map`이라는 단어와 실제 작동하는 기능이랑은 좀 괴리가 있어서 한번에 이해하기가 어려웠던 고차함수였다.

일단 `filter`와 같이 Collection Type에서 지원하는 함수이자 클로저다.

Array 등에서 가져온 요소들을 개발자가 내보내고 싶은 Type으로 바꾸면 된다.

[String]을 [Int]로, 혹은 그 외 다른 Type으로.

즉, 간단히 압축, 요약하자면 Array 등의 Collection Type의 요소를 새로운 Type으로 바꾸어 새로운 Array로 내보내는 것이다.

<br/>

## 사용법

애플 공식문서에 나온 예제다. 클로저를 축약해서 더 간결하게 보여준다.

대문자가 섞여있는 `cast` Array를 전부 소문자로 바꾸어 `lowercaseNames` Array로 만들었다.

```swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]
let lowercaseNames = cast.map { $0.lowercased() }
// 'lowercaseNames' == ["vivien", "marlon", "kim", "karl"]

let letterCounts = cast.map { $0.count }
// 'letterCounts' == [6, 6, 3, 4]
```

단순히 소문자로 바꾼 것이기 때문에 `lowercaseNames`의 Type을 보면 기존의 `cast`와 같은 타입인 [String]임을 알 수 있다.

![using map result1](/img/2022-12-17/map_1.png){: width="80%"}

_Apple Documnet의 map 예제_

<br/>

두번째 코드를 보면 이번엔 `for-in`구문으로 `map`과 똑같은 기능을 하도록 만든 것이다.

이 말인즉슨, `map`은 내부적으로 `for-in`과 거의 흡사하게 동작한다는 점이다.

```swift
var letterCounts:[Int] = []
cast.forEach { str in
    let strCount = str.count
    letterCounts.append(strCount)
}
// `letterCounts` == [6, 6, 3, 4]
```

차이가 있다면 아래와 같다.

- 재사용성의 용이함
- 코드의 간결함
- 외부 변수로 부터의 안정성

이걸 `map`으로 바꾸어 구현하면 `letterCounts`의 Type이 [Int]로 바뀐 것을 볼 수 있다.

![using map result2](/img/2022-12-17/map_2.png){: width="80%"}

_Apple Document의 map 예제2_

추가적으로, 고차함수를 이용하는 것이 `for-in`구문을 이용한 것보다 컴파일러 성능이 더 좋다고 한다.

<br/>

## 출처
- [Apple Documnet For Map](https://developer.apple.com/documentation/swift/array/map(_:)-87c4d)