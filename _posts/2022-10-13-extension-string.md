---
title: iOS, Swift 문자열에서 인덱스를 정수로, 문자 추출하기!
date: 2022-10-13 09:25:00 +0900
categories: [Swift, Util]
tags: [swift, util]
author: WalterCho
---

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

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.