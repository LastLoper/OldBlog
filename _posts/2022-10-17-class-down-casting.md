---
title: iOS, Swift Down Casting 이해하기 
date: 2022-10-17 23:00:00 +0900
categories: [Swift, Class, DownCasting, OOP]
tags: [swift]
author: JohnCoder
---

# 다운캐스팅!

객체지향 언어를 공부하면서 가장 헷갈렸던 개념이 아니였던가 싶다. 상속까지는 어찌저찌 이해를 했는데
상속이 되면 부모/자식 클래스 인스턴스에서 어떤 특성이 생기는지 이해하는데 참 많은 시간이 걸렸다.

상속의 개념을 가장 잘 설명하는, 또는 부모/자식 클래스를 넘나들면서 객체지향 패러다임을 잘 활용할 수
있는 개념이 다운캐스팅이라 생각하여 본 포스팅을 쓰게 되었다.

다운캐스팅을 설명하기 위해서는 클래스간 상속이 전제가 되어야 한다. 상속된 클래스는 유용한 특성이
너무나도 많기 때문에 본 포스팅에선 상속에 대해 어느 정도 안다고 생각하고 시작하겠다.

## 코드 예시 정의!
모든 클래스/상속에 대한 포스팅이 그렇듯이, 적절한 예시로 시작을 하고자 한다.
한국과 미국에 스마트폰을 팔아먹는 제조업자 컨셉으로 설명을 시작하겠다.

먼저, 한국과 미국에 파는 스마트폰에 대해 기능 정의를 먼저 하고, 코드로 넘어가보자

***공통기능: 언어, 전화기능*** <br>
***한국: 공통 기능 + 카메라 셔터음***<br>
***미국: 공통 기능 + 녹음 기능***

일단, 공통 기능과 한국과 미국에 특화된 기능들을 정의했다. 그리고, 이를 생산해서
테스트 까지 해보는 예시 코드를 준비해 보았다.


```swift

//공통 기능 클래스 정의
class CommonFunction { 
    
    var language: String = "" //디바이스 언어
    
    func call() { //전화 기능
        print("Common Call")
    }
}

class ToKorea: CommonFunction { //공통기능 상속 & 한국 폰 기능 정의

    override func call() { //공통 기능의 전화 기능을 한국에 맞게 정의
        print("Call in korean")
    }
    
    func cameraShutterSound() { // 한국 전용 카메라 사운드 정의
        print("Chal-kak!")
    }
}

class ToUSA: CommonFunction { //공통기능 상속 & 미국 폰 기능 정의

    override func call() { //공통 기능의 전화 기능을 미국에 맞게 정의
        print("Call in English")
    }
    
    func voiceRecording() { //녹음 기능
        print("recording")
    }
}

```

자, 이제 기능이 대한 정의를 클래스로 표현하였고, 이를 어떻게 다운캐스팅에 접목해서 사용할 수 있는지
살펴보자.

## 휴대폰 품질검사 한번 해봅시다!

자 여기서, 프로세스를 하나 추가하자. 

```swift

//샘플을 품질 검사하는데로!
var qualityChecking: [CommonFunction] = []

let toKorea1: ToKorea = ToKorea()
let toUSA1: ToUSA = ToUSA()
    
qualityChecking.append(toKorea1)
qualityChecking.append(toUSA1)


```

미국 전용 휴대폰과 한국 전용 휴대폰을 각각 1대씩 샘플로 선정하여 품질 검사를 실시하려고 한다.
 
qualityChecking 이라는 변수를 하나 선언해서, CommonFunction(공통 기능)을 요소로 하는
배열 타입을 일단 빈 배열로 선언하자.

그리고, toKorea1, toUSA1 이라는 완제품을 하나씩 선정하여 인스턴스를 선언하여, 
품질 검사 샘플 배열에 append하여 추가를 한다.

이 때, 우리가 확인할 수 있는 것이 분명 품질 검사 배열은 CommonFunction 타입으로 되어있는데, 
어떻게 ToKorea, ToUSA를 요소로 append가 가능한거지? 라는 의문을 가질 수가 있다.

이 때, 나오는 개념이 업캐스팅이다. 부모 클래스의 타입에, 자식 클래스를 대입하는 걸 업캐스팅이라 하는데,
부모 클래스 타입에 자식 클래스 인스턴스를 대입할 수 있는 이유가, 자식 클래스는 부모 클래스의 특성을
전부 가지고 있기 때문에 대입이 가능하다! 

그렇기 때문에, 
***업캐스팅은 무조건 성공한다*** <br>
라고 생각하면 된다 

```swift
var son: ToKorea = ToKorea()
var parents: CommonFunction = son as CommonFunction
```
이랑 똑같은 말인데, son 뒤에 as CommonFunction을 굳이 붙이지 않아도, 암시적으로
parent에 son 변수 대입이 가능해진다. 그래서, 캐스팅이라는 개념은 그렇게 중요하게 다루지 않는다.
(그냥 되는거니까, 굳이 이름 붙여서 헷갈리지 말자구요!)

자, 다시 본론으로 돌아와서
우리는 한국폰1, 미국폰1을 하나씩 샘플로 가져와서 품질 검사를 하도록 넘겼다.
품질 검사하는 배열의 타입이 CommonFunction인데, 과연 여기서 다운캐스팅이 어떻게 쓰였는지
다음 코드를 보자!

```swift

for sample in qualityChecking {
    if let temp = sample as? ToKorea { //다운캐스팅
        temp.call()
        temp.cameraShutterSound()
    }
    if let temp = sample as? ToUSA {
        temp.call()
        temp.voiceRecording()
    }
}
```

for - in 구문으로, 샘플로 들어온 배열의 요소를 하나씩 검사해보는 코드다. 당연히 sample은 
CommonFunction 타입이고, 이를 어떻게 ToKorea, ToUSA의 특성을 가져와서 검사를 하는지 보자.

반복문 안에 코드를 잘 보면, if let 구문이 있다. 이 조건으로

temp = sample as? ToKorea 라고 코드가 들어가있는데, 이는

즉, CommonFunction 타입의 sample 변수를 ToKorea 타입으로 다운캐스팅(자식->부모)
하여 성공하면 temp 타입에 대입하도록 한다는 뜻이다.

(그럼 암시적으로 temp변수의 타입은 ToKorea 로 지정이 되겠지?)

***temp: ToKorea = sample as? ToKorea ***

로 풀어서 쓸 수 있으며, 이 다운캐스팅이 성공한 이유는, 배열에 들어간 요소의 타입은 CommonFunction
(부모클래스) 이지만, 실제로 갖고있는 인스턴스는 ToKorea, ToUSA(자식 클래스) 이기 때문이다.

위 코드를 실행해보면, 

```swift
//<결과>
// Call in korean
// Chal-kak! 
// Call in English
// recording

// 한국폰도 잘작동, 미국폰도 잘작동!!
```

따라서, 다운캐스팅이 되는 조건은

***부모 클래스의 타입에 자식 클래스의 인스턴스를 대입했고, 그리고 그 부모 클래스 타입의 변수를 *** <br>
***이용하여 자식 클래스에 접근하고자 하는게 다운캐스팅이다! ***

를 설명하기 위해 장황하게 글을 썼다.
진짜, 진짜 위에 글을 너무 읽기 싫다면 바로 위에 다운캐스팅 조건으로 강조해놓은 두줄만 읽어도 
된다. 

상속이라는 개념과 as, as?, as! 캐스팅 연산자를 선행해서 알아야 코드 이해에 어려움이 없을 것 같다.
장황하게 설명하기 위해서 상속도 조금 다룰까 했지만, 다운캐스팅을 검색한 것 자체가 상속이란 개념을
이미 알고있는 상태라 생각하여 생략하였다.

다음 포스팅에서는 내가 초보자였을때 힘들었던 또 다른 주제를 가지고 나타나도록 하겠다!
