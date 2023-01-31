---
layout: page
menubar: swift_menu
title: 애플의 강력한 프로그래밍 언어, Swift
subtitle: Fast, Safe, Expressive
show_sidebar: false
toc: false
hero_image: /img/swift/swift_hero2.png
hero_height: is-medium
---

애플 WWDC14에서 공개한 프로그래밍 언어 Swift.

기존의 Objective-C의 단점을 보완하고 컴파일러를 LLVM으로 변경한 현대적인 프로그래밍 언어다.

애플에서는 초기에 빠르고(Fast), 현대적이며(Modern), 안전한(Safe), 상호작용이 훌륭한(Interactive) 언어의 특징으로 Swift를 설계했으나 오픈 소스로 전환, 출시하면서 빠르고(Fast), 안전하며(Safe), 표현성이 좋은(Expressive) 언어의 특징을 가지고 있다.

어느정도 하위 안정성을 보장하고 있어 Objective-C 코드를 혼용해서 프로그래밍할 수 있다.

<br/>

## 언어적 특징

### 1. 신속성(Fast)

- C언어에서 파생된 언어가 Objective-C이고, 이 Objective-C를 보완한 언어가 바로 Swift다.
- '오브젝트 씨'라고 알고 있는 사람이 제법 많은데 정확하게는 '오브젝티브 씨'다. 줄여서 옵젝씨, 오브젝씨라고 말한다.
- LLVM 컴파일러로 Python의 약 220배, Objective-C의 약 1.5배, C언어와 비슷한 성능이라고 한다.

<br/>

### 2. 안정성(nil-Safe)

- 스위프트는 안전한 프로그래밍을 지향한다. 이 말인즉슨, nil값이 들어가는 등의 스멜코드를 허용하지 않는다는 뜻이다.
- 옵셔널과 빠른 탈출 구문들이 대표적인 예, guard let 문이나 if let 구문등으로 nil값에 대한 처리를 말한다.
- 그 외에도 현대적인 프로그래밍 언어답게 오류처리, 타입 통제등은 개발자가 할 수 있는, 어쩌면 프로그램이 문제를 일으킬 수 있는 사소한 오류들을 방지할 수 있도록 해준다.

<br/>

### 3. 더 나은 표현성(Expressive)

- 가독성이 좋다고 알려진 Python의 문법을 채용해서 간경하고 보기 편하며, 애플은 더 나은 표현을 지속적으로 연구, 보완하고 있다.
- 절차/객체지향, 함수형 프로그래밍 등의 다양한 프로그래밍 패러다임을 채택하고 있다.

<br/>

### 4. 프로토콜 지향 (Protocol)
- Swift는 버전 2.0에서 대부분의 Type이 Class에서 Struct로 변경 되었다.
- 하지만 Struct는 상속이 불가능 하다는 점에서 기존의 기능들을 사용하기 위해 Protocol이 채택되었다.
- 결론적으로 다중 상속이 불가능하던 문제를 해결했으며, 더 나은 추상화 매커니즘의 구현이 가능해졌다.
 
<br/>

### 5. 확장 (Extention)
- Protocol이 제 기능을 완벽하게 수행하기 위해서 같이 등장한 기능이다.
- Protocol을 선언했다면 반드시 해당 Delegate를 위임해야 하고, 이는 클래스를 확장 (Extention)해서 사용해야 한다.
- Protocol과 Extention의 조합으로 인해 유지보수와 재사용 측면이 강화되었다.

<br/>

## 출처
- [나무위키](https://namu.wiki/w/Swift)
- [LLVM 컴파일러](https://namu.wiki/w/LLVM)
- [Objective-C](https://namu.wiki/w/Objective-C)