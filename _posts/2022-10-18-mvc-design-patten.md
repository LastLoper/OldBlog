---
title: iOS 코딩으로 보는 MVC 디자인 패턴
date: 2022-10-19 21:25:00 +0900
categories: [프로그래밍, 개념]
tags: [ios, 프로그래밍, 개념, 디자인패턴, MVC]
author: WalterCho
---

## 디자인 패턴이란?
프로그램의 구조를 단순화하고 유지보수하기 쉽게 하기 위한 하나의 전략이다. 프로그램은 결국 사용자가 보는 뷰(View)와 데이터(Data)가 결합된 형태다. 


### 목표
- 기술 부채의 최소화
- 모듈화를 통한 재사용 및 지속 가능한 코드를 만드는 것
- 가능한 책임을 최대한 분산시키는 것

## MVC
Storyboard 를 쓰던 UIKit 에서는 애플에서도 MVC패턴을 사용하도록 권장했다.
모델(Model), 뷰(View), 컨트롤러(Controller) 이렇게 3가지를 나타내는 약어이다.

모델은 앱 내에서 사용되는 데이터 구조, class 또는 struct로 만든다.
```swift
struct GameModel {
    var title: String
    var platform: String
    var launchingDate: Date
}
```

뷰는 UI적인 요소들을 말한다.
컨트롤러는 뷰와 모델을 연결해주는 중개자의 역할

## 장점
뷰와 모델의 의존성이 없다.

## 단점
컨트롤러가 너무 무거워질 수 있고 책임이 많아진다.

## iOS 예제
보통 파일구조를 다르게 만든다.

