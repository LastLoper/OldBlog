---
title: iOS 스위프트 GCD, DispatchQueue 쉽게 이해하기
date: 2022-09-04 18:48:00 +0900
categories: [Swift, DeepDive]
tags: [swift, gcd]     # TAG names should always be lowercase
author: WalterCho
---

쓰레드는 조금 무거운 주제라 지겨울 수도 있겠지만, 별 수 있을까<br>
그래도 한 번 잘 읽고, 정리하면 스레드가 조금 더 가까워질 것이라 확신한다.

## GCD?
Grand Central Dispatch
쉽게 말해 스위프트에서 Queue를 관리하는 API다.<br>
즉, 메인 스레드 또는 백그라운드 스레드를 관리하는 것. 애플에서도 DispatchQueue를 아래와 같이 정의하고 있다.

***An object that manages the execution of tasks serially or concurrently on your app's main thread or on a background thread.***<br>
***앱의 메인 스레드 또는 백그라운드 스레드에서 순처적인 작업 혹은 병렬적 작업들을 관리하는 객체***

```swift
DispatchQueue.main              //UI를 담당하는 메인 스레드
DisaptchQueue.global()          //백그라운드에서의 작업을 담당하는 스레드
```

사용법은 단 두가지밖에 없어서 딱히 어려운 것은 없다. 비동기 처리를 위해서는 **DispatchQueue.global()**만 써주면 되니까. 하지만 난 효율의 민족이니까 효율적으로 잘~ 쓰기 위해서 조금 더 공부했다.

## DispatchQueue.global() 사용법
**DispatchQueue.main**는 사용자와의 상호작을 위한 UI스레드로써, 메인 스레드라고도 불리며 UI가 Lock이 걸리는 일이 발생하면 안된다. 그래서 다운로드 같은 네트워크 통신은 Xcode에서도 웬만하면 **DispatchQueue.global()**에서만 하도록 반쯤 강제한다.

또한 대부분의 경우 **DispatchQueue.global()**을 훨씬 더 많이 쓰기 때문에 이에 대한 내용을 위주로 써보려고 한다.

### Sync와 Async, 동기와 비동기
```swift
//'.sync'와 '.async'로 구분해서 사용할 수 있다.
//'동기 처리', '비동기 처리'가 바로 이것이다.
DispatchQueue.global().sync { /* task */ }
DisaptchQueue.global().async { /* tasks */ }
```

이제 작업을 백그라운드 스레드로 넘겨서 실행하기 때문에 메인 스레드에는 아무런 영항이 없다. 온전히 다른 스레드에서 별개의 동작을 하는 것이다.<br> 하지만! 백그라운드 스레드에서도 **응답을 주고 받으면서 작업할 것인지**, 아니면 **응답과는 상관없이 독립적으로 작업할 것인지**를 구분해야 한다.

이번 포스팅에서는 조금 더 많이 쓰는 .async(비동기 처리)를 조금 더 자세하게 정리했다.

### .Async 예제
```swift
DispatchQueue.global().async {
    sleep(2)
    print("백그라운드에서 실행됩니다.")
}
print("메인스레드의 작업이 실행됩니다.")

//결과
메인스레드의 작업이 실행됩니다.
백그라운드에서 실행됩니다.
```

DispatchQueue.global()로 넘어간 작업은 메인 스레드에 아무런 영향을 주지 않고, .async가 사용되었기에 **비동기적으로 처리**된다. 따라서 메인 스레드에 있는 print() 구문이 먼저 출력되고, 백그라운드 스레드로 넘어간 print() 구문이 출력되는 것을 볼 수 있다.

하지만 이는 sleep(2)라는 딜레이로 인해 늦게 메인 스레드보다 늦게 출력된 것이지, 항상 백그라운드 스레드가 늦게 끝난다는 보장은 없다. 만약 백그라운드 스레드로 넘어간 작업이 먼저 끝났다면, 메인 스레드보다 먼저 실행될 수 있다.

Sleep()함수가 없는 아래 코드를 실행하면 백그라운드 스레드의 print() 구문이 먼저 실행되는 것을 볼 수 있다.
```swift
DispatchQueue.global().async {
    print("백그라운드에서 실행됩니다.")
}
print("메인스레드의 작업이 실행됩니다.")

//결과
백그라운드에서 실행됩니다.
메인스레드의 작업이 실행됩니다.
```

### QOS라는 것도 있던데?
DispatchQueue.global()을 쓸 때, 한 가지 생각해야 할 부분이 있다. 바로 **QOS(Quality-Of-Service)**다.<br> 
말 그대로 백그라운드에서 작업할 때 얼마나 중요한 작업인지를 시스템에 알려주는 것으로 생각하면 된다.

QOS 값으로는 enum class로 정의되어 있다. 상황에 맞는 파라미터를 써주자
- userInteractive
- userInitiated
- default
- utility
- background
- unspecified

하나씩 살펴보자
#### 1. userInteractive
애니메이션이나 어떤 이벤트로 인해 UI를 업데이트 하고 싶다면, 이 값으로 즉각적인 응답을 요청할 수 있다.

#### 2. userInitiated
어떤 이미지를 받아와서 보여줘야 하는데, 시간이 걸린다면 이 값으로 요청하고 로딩 애니메이션을 보여줄 수 있다.<br>
다시 말하자면, 빠른 응답을 요구하면서도 사용자를 기다리게 만드는 요소인 것이다.

#### 3. default
qos 파라미터를 넣어주지 않으면 동작하는 가장 기본적인 우선순위다.

#### 4. utility
사용자가 제어할 수 없는 우선순위이며, 응답이 없어도 앱 사용에는 아무런 지장이 없는 요청 값이다.

#### 5. background
제일 낮은 우선순위로써, 앱이 백그라운드 상태에 있을 때 동작한다.

#### 6. unspecified
Document에는 정의되어 있지만 실제로는 쓰지 않는다.

## 백그라운드 스레드 종료 시점
종종 네트워크 작업이 필요할 때, 각기 다른 API를 사용해야 할 때가 있다. 그리고 그 모든 작업이 끝나야지만 UI를 업데이트 해야하는 경우가 있다. 이때 개발자는 백그라운드 스레드의 종료 시점을 어떻게 알 수 있을까?<br>
간단하게 **DispatchGroup()**을 써주면 된다.

### 스레드가 모두 종료되기까지 기다려야 하는 Wait()
```swift
let queue = DispatchQueue.global()
let group = DispatchGroup()

queue.async(group: group) {
    sleep(3)
    print("작업 1")
}

queue.async(group: group) {
    sleep(5)
    print("작업 2")
}

group.wait()
print("모든 작업이 완료되었습니다.")
print("메인 스레드를 시작합니다.")

//결과
작업 1
작업 2
모든 작업이 완료되었습니다.
메인 스레드를 시작합니다.
```

위 예제와 같이 wait()을 써주면 백그라운드 스레드의 종료시점을 알 수 있다.<br>
하지만 결과에서 볼 수 있듯이 메인 스레드 작업이 가장 늦게 끝난다.<br> 
다시 말해, **백그라운드 스레드 작업이 종료되는 시점까지 Rock을 거는 것**이다.

### 스레드가 종료되는 시점을 알려주는 Notify()
```swift
group.enter()
queue.async(group: group) {
    sleep(3)
    print("작업 1")
    group.leave()
}

group.enter()
queue.async(group: group) {
    sleep(5)
    print("작업 2")
    group.leave()
}

group.notify(queue: queue) {
    DispatchQueue.main.async {
        //UI 작업
    }
    print("모든 작업이 완료되었습니다.")
}
print("메인 스레드를 시작합니다.")

//결과
메인 스레드를 시작합니다.
작업 1
작업 2
모든 작업이 완료되었습니다.
```
Notify()를 사용하면, 이제야 비로소 백그라운드 스레드다운 모습을 보여주면서도 종료 시점까지 확인할 수 있다.