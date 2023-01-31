---
layout: post
menubar: ios_menu
date: 2022-10-10 21:22:00 +0900
title: iOS, Swift Date 이해하기 1편
subtitle:  그러니까...
categories: property-observer
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

앱을 구현하다가 날짜 관련된 데이터를 다룰 일이 있어서
찾아보다가 공부한 내용을 담아보고자 한다.

물론 공식 문서만 봐도 이해가 될 만큼 잘 나와있지만, 나같이 성질 급한 사람들을 위해 쓰는 글이고,
삽질한 경험을 토대로, 어떻게 삽질을 했는지 경험을 공유하고자 포스팅을 하게 되었다. 

## Date?

Date는 날짜 및 시간을 다루는 Swift 의 데이터 형이다. (Int, Double 처럼 구조체로 된 데이터형!)

```swift
struct Date
```

Apple Developer 문서에 따르면

***A Specific Point in time, independent of any calendar or time zone***
***즉, 날짜나 어떤 위치(즉, 한국인지 미국인지 어디인지)와 전혀 무관한 시간의 특정 지점***

이 말이 너무 이해가 안갔다. 아니 난 한국에있는데 왜 Date타입으로 날짜를 확인하면 자꾸 전날이 뜨거나
시간이 -9시간 되서 뜨는거야? 이 정도는 코드나 운영체제에서 알아서 위치를 확인해서 해주지않나..?
아니 그럼 시차는 Date 인스턴스에서 어떻게 적용하는 걸까...? 
혼자 엄청나게 고민을 했다.

저 설명을 쉽게 표현하자면

***Date형은 UTC +0기준. 즉 아무런 시차도 적용되지 않은 시간 기준이다!!!***

어떤 Date 인스턴스가 가지는 날짜/시간은 UTC +0 기준이기 때문에 시차는 아예 적용되어 있지 않다.
(검색해보니 UTC와 GMT 는 거의 비슷하게 통용된다고 한다.(GMT는 그리니치 천문대 표준시))

## Date의 요소

Date가 가지는 주요 요소를 보면

1) 연도
2) 월
3) 일
4) 시간
5) 분
6) 초
7) 밀리초
8) 요일

정도가 가장 많이 쓰일 것 같다.

(물론 다른 요소들도 많은거 같다... 주차, 분기, 등..)
기본적으로 날짜/시간 관련되서는 전부 가지고 있다.

## Date 생성자!

위에서 설명했듯이, Date 객체 자체는 UTC 기준의 시간만을 가지는 것만 알면 헷갈리지 않고
사용할 수 있을것이다.

Date 타입도 구조체기 때문에 생성자를 갖는다. Date는 어떤 생성자를 가지고 어떻게 속성들을 가지게 
되는지 알아보자.

### 1) init()
그냥 Date()로 생성시키면 생성되는 현재 시각이 표시된다.
(뒤에 +0000에서 UTC 기준 시간임을 알 수 있다.)

```swift
let currentDate: Date = Date()
//2022-09-19 12:47:30 +0000 
```

### 2) init(timeIntervalSinceNow: TimeInterval)

TimeInterval 파라메터의 타입은 정수형(Int)인데, 현재 시간에서 TimeInterval 초 만큼 지난 시간이 
세팅된다. 아래 코드를 보면 현재 시간은 12:50:00인데, 30을 부여하면 12:50:30으로 표시된다.

```swift
let currentDate: Date = Date()
print(currentDate)
//2022-09-19 12:50:00 +0000

let date2: Date = Date(timeIntervalSinceNow: 30)
print(date2)
//2022-09-19 12:50:30 +0000
```

### 3) init(timeInterval: TimeInterval, since: Date)

since 파라메터에 Date 요소가 들어가게 되어있다. 즉, 파라메터로 들어간 Date 시간으로 부터 TimeInterval 에 세팅된 초 만큼 지난 시간을 객체에 가진다. 
아래 코드를 보면 현재시간 + 30초된 시간이 date2 변수에 저장되어 12:56:15가 세팅되었고,
date3에 since에 세팅된 date2 기준에서 43초 흐른 시간이 세팅되어 있다.

```swift
let date2: Date = Date(timeIntervalSinceNow: 30)
print(date2)
//2022-09-19 12:56:15 +0000

let date3: Date = Date(timeInterval: 43, since: date2)
print(date3)
//2022-09-19 12:56:58 +0000
```

### 4) init(timeIntervalSinceReferenceDate: TimeInterval)

2001년 1월 1일 0시부터 TimeInterval 초 만큼 흐른 시간이 세팅된다.

```swift
let date4: Date = Date(timeIntervalSinceReferenceDate: 45)
print(date4)
//2001-01-01 00:00:45 +0000
```

### 5) init(timeIntervalSince1970: TimeInterval)

1970년 1월 1일 0시부터 TimeInterval 초 만큼 흐른 시간이 세팅된다.
아래 코드에서는 1970년 1월 1일 0시0분0초부터 5초가 흐른 시간이 세팅되었다.

```swift
let date5: Date = Date(timeIntervalSince1970: 5)
print(date5)
//1970-01-01 00:00:05 +0000
```

Date 타입의 생성자에 따라서 어떻게 날짜/시간 속성을 가지는지 알아보았다. 
Date타입도 구조체기 때문에 Date 인스턴스 간 비교 연산도 가능하다.

깨알팁) 레퍼런스 문서에 func == (lhs: Date, rhs:Date), func > (lhs: Date, rhs:Date) 로 표시되어 있는
메서드들은 인스턴스간 비교연산자로 Bool연산이 가능하다는 뜻이다.
(물론 +, - 같은 계산도!) 


다음 포스팅에서는 이 Date 타입을 어떻게 구워삶는지 알아보자.