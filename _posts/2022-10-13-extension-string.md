---
title: 문자열에서 인덱스를 정수로, 문자 추출하기!
date: 2022-10-13 09:25:00 +0900
categories: [Swift, Util]
tags: [swift, util]
author: WalterCho
---

<!-- C, 자바, 코틀린등 다른 프로그래밍 언어에서는 문자열에서 인덱스를 정수로 주고 문자를 추출할 수 있다.<br>

```kotlin
var str: String = "Loperz"
println(str[0])
//L
```
-->

String을 확장(Extension)해서 C, 자바, 코틀린등과 같이 사용할 수 있다.

```swift
var greeting = "Hello, Loperz"
print(greeting[0])

extension String {
    //Character형태로 반환
    subscript(_ idx: Int) -> Character {
        return self[self.index(self.startIndex, offsetBy: idx)]
    }
}

//H
```