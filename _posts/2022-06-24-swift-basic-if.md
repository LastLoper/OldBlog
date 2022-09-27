---
author: WalterCho
title: Swift 참과 거짓을 구분하는 If문 이해하기
date: 2022-06-24 13:15:00 +0900
categories: [Swift, Basic]
tags: [swift, basic]     # TAG names should always be lowercase
image:
  path: /post_img/0624/swift_if_1.png
  width: 972
  height: 972
  alt: if문의 작동 원리에 대한 이해를 돕는 그림
---

## 개요
이름부터 아주 직관적인, 주어진 `값이 참인지 거짓인지를 판별`하는 조건문이다.

사용법 또한 간단하다.

## 도식화
![diagram](/post_img/20220624/swift_if_2.png){: width="972" height="972" style="max-width: 95%" .shadow }
_if문 도식화_

## 사용법
```swift
//기본적인 형태로써, 참과 거짓을 판별한다.
var isOn = true
if isOn {
    print("전원이 켜졌습니다.")
} else {
    print("전원이 꺼졌습니다")
}

// => 전원이 켜졌습니다.
```

```swift
//변수와 상수, 변수와 변수를 == 연산자로 비교할 수 있다.
var isTen = 9
let eleven = 11
if isTen == 9 {
    print("9입니다.")
} else if isTen == eleven {
    print("11입니다.")
} else {
    print("10인지 모르겠어요.")
}

// => 10인지 모르겠어요
```

```swift
//문자열 역시 == 로 비교할 수 있다.
var food = "삼겹살"
If food == "삼겹살" {
    print("소주 한 병 주세요~")
} else if food == "치킨" {
    print("시원한 맥주 주세요~")
} else {
    print("사이다 주세요~")
}

// => 소주 한 병 주세요~
```

## Tip. 실무에 많이 쓰이는 방법
대게 If - else if - else의 기본 형태를 가장 많이 쓰지만 Swift에서는 If문을 3항 연산자로 사용할 수 있다.

그래서 아래와 같은 형태로도 제법 쓰고 있다.

```swift
var isToday = "2022-06-24"
let todayLabel.isHidden = isToday == "2022-06-24" ? false : true

// todayLabel.isHidden은 false의 값을 가지게 되고, 화면에 드러나게 된다.
```