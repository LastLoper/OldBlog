---
layout: page
menubar: control_flow_menu
title: if - else if - else
subtitle: 참과 거짓을 구분하는 조건문
show_sidebar: false
toc: false
hero_image: /img/control-flow/if-hero.png
hero_height: is-medium
hero_darken: true
---

이름부터 아주 직관적인, 주어진 `값이 참인지 거짓인지를 판별`하는 조건문이다.

<br/>

예를 들어보자,

내가 맛있는 🍎를 들고 있을 때, 

누군가 "그거 사과야?" 라고 물어본다면 내가 할 수 있는 대답은 `맞아`또는 `아니야`라고 대답할 것이다.

<br/>

`if`문이 바로 이것이다. 

어떤 질문을 했을 때 그것이 `참인지 거짓인지`만을 대답할 수 있고, 그 이후의 행동을 결정한다.

<br/>

## 사용법

기본적인 형태로써, if - else 의 구조를 가진다.

```swift
let 🍎 = "사과"
if 🍎 == "사과" {
    print("응, 맞아. 하나 먹어볼래?")
} else {
    print("사과 아니야")
}
// 응, 맞아. 하나 먹어볼래?
```

<br/>

else if 구문을 `중간에` 넣어서 조건을 더 세분화 할 수 있다.
else 구문은 항상 마지막이어야 한다.

```swift
let 🍌 = "바나나"
if 🍌 == "사과" {
    print("응, 맞아. 하나 먹어볼래?")
} else if 🍌 == "바나나" {
    print("이건 바나나야!")
} else {
    print("이건 사과도 아니고 바나나도 아니야.")
}
// 이건 바나나야!
```

<br/>

### 논리 연산자

논리 연산자(`&&`, `||`)를 사용해서 두 개 이상의 조건을 한 줄로 작성할 수 있다.

```swift
//&&(and 연산자)는 둘 모두 참이어야 한다.
let 🍎 = "사과"
if 🍎 == "사과" && 🍎 != "채소" {
    print("맞아, 사과는 과일이지, 채소가 아니지!")
} else {
    print("글쎄 잘 모르겠는데..?")
}
// 맞아, 사과는 과일이지, 채소가 아니지!

//||(or 연산자)는 둘 중 하나만 참이어도 된다.
let 🍎 = "사과"
if 🍎 == "사과" || 🍎 != "과일" {
    print("과일인지는 모르겠지만.. 일단 사과는 맞아!")
} else {
    print("글쎄 잘 모르겠는데..?")
}
//과일인지는 모르겠지만.. 일단 사과는 맞아!
```

<br/>

### 3항 연산자

Swift에서 아래와 같이 if문을 3항 연산자로 사용할 수 있다.

if - else 문을 간단하게 표현한 것이다.

```swift
var isToday = "2022-06-24"
let todayLabel.isHidden = isToday == "2022-06-24" ? false : true
```