---
layout: page
menubar: rxswift_menu
title: 어려운 RxSwift! Observable 함수 정리
subtitle: 동기/비동기 처리를 간편하게
show_sidebar: false
---

## Observer 선언
Observable은 선언만 해서는 동작하지 않는다. 말 그대로 '관측가능한' 상태이기 때문이다.<br>
반드시 구독(subscribe)을 해야만 작동한다. 아무도 보지 않는데 동작할 이유가 없다.

### Just
단 1개의 요소만 방출하는 함수다.
```swift
Observable<Int>.just(1)
```

### Of
1개 이상의 요소들을 방출하는 함수다.<br>
쉼표로 구분해서 나열하거나 배열을 방출할 수도 있다.
```swift
//여러개의 요소를 방출하는 Obsevable Sequence
Observable<Int>.of(1, 2, 3, 4, 5)
//1
//2
//3
//4
//5

//배열을 방출하며 이때 타입은 <Array>가 된다.
//1개의 배열을 방출한다는 점에서 just와 같은 기능을 한다.
Observable.of([1, 2, 3, 4, 5])
//[1, 2, 3, 4, 5]
```

### from
Array형태만 인자로 받아 각 요소를 방출한다.
```swift
Observable.from([1, 2, 3, 4, 5])
//1
//2
//3
//4
//5
```

## Subscribe 사용법
가장 처음에도 언급했지만 Observable은 그냥 '관측이 가능한' 상태이기 때문에 Observer로 관측을 하거나, subscribe해야만 작동한다.<br>

```swift
//onNext를 명시하는 가장 일반적인 사용법이다.
Observable.of(1, 2, 3)
    .subscribe(onNext: {
        print($0)
    })
//1
//2
//3

//onNext를 명시하지않고 간략하게 표현할 수도 있다.
//이때는 onNext이벤트 및 onCompleted이벤트까지 출력된다.
Observable.of(1, 2, 3)
    .subscribe {
        print($0)
    }
//next(1)
//next(2)
//next(3)
//completed

//Observable에 요소가 있는지 확인 할 수 있다.
Observable.of(1, 2, 3)
    .subscribe {
        if let element = $0.element {
            print(element)
        }
    }
//1
//2
//3
```

## Observable 구독 취소
'관측 가능한 상태'인 Observable을 구독했다면, 반대로 취소도 가능해야 한다.<br>
subscribe의 Return형태가 바로 Disposable인 이유가 여기에 있다. 그래서 이 return에 대한 처리가 바로 dispose()이다.<br>
이때, 각각의 Observable에 대한 메모리 할당을 해제할 수도 있지만, DisposeBag으로 일괄적으로 처리할 수도 있다.

```swift
//방출 후 dispose로 Observable을 종료한다.
Observable.of(1, 2, 3)
    .subscribe(onNext: {
        print($0)
    })
    .dispose()      //명시적으로 이 subscribe를 종료, 메모리 해제하는 것이다.

//disposeBag 객체로 모아서 할당된 메모리를 헤제한다.
let disposeBag = DisposeBag()
Observable.of(1, 2, 3)
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)      //disposeBag에 넣어두고, DisposeBag객체가 해제될 때
```

### Dispose()와 DisposeBag()
subscribe의 return type은 disposable이기 때문에 subscribe의 끝에는 dispose()를 해주어야 한다.<br>
**말 그대로 구독을 종료하는 것**이다.
그리고 dispose() 함수를 쓰면 그 즉시 구독이 해제된다. 더이상 구독이 불가능한 상태가 되는 것이다. 그럼 구독한 이유가??

반면, DisposeBag() 객체가 메모리에서 해제될 때까지 구독을 유지하게 된다.<br>
다시 말하면, 구독이 필요없어지는 순간이 바로 DisposeBag 객체가 메모리에서 해제되는 순간이란 의미이다.

따라서 dispose()로 구독을 바로 종료하는 경우는 거의 없다.

## 어떤 요소도 방출하지 않는 함수
### empty
요소가 없는 Sequence를 방출한다.<br>
언제 쓸 수 있을까?<br>

- 의도적으로 0개의 값을 가진 Observable을 Return하고 싶을 때
- 즉시 종료되는 Observable을 Return하고 싶을 때

```swift
//Count가 0인 Obsevable이며 onCompleted 외에는 어떠한 값도 방출하지 않는다.
Observable.empty()
    .subscribe {
        print($0)
    }


//Type을 지정해주면 이벤트를 출력한다.
Observable<Void>.empty()
    .subscribe {
        print($0)
    }
//completed

//위와 예시와 같은 결과를 출력한다.
Observable<Void>.empty()
    .subscribe(onNext: { },
    onCompleted : {
        print("completed")
    })
//completed
```

### never
empty가 count 0인 Observable을 방출하고 정상적으로 종료된다면, never는 어떠한 Sequence도 방출하지 않고 종료도 안되는 함수다.<br>
언제 쓰지..?
```swift
Observable<Void>.never()
    .debug("never")         //굳이 확인하고 싶을 때 .debug()를 사용해서 작동하는 것을 확인할 수는 있다.
    .subscribe(onNext: {
        print($0)
    },
    onCompleted: {
        print("completed")
    })
```

## Create
Observable이 직접 Sequence를 만드는 함수다.<br>
```swift
//값을 방출하는 Sequence를 직접 만든다.
//Disposable을 반환하는 Observable을 방출, subscribe가 출력한다.
Observable.create { observer -> Disposable in
    observer.onNext(1)
    observer.onNext(2)
    return Disposables.create()
}
.subscribe {
    print($0)
}
.disposed(by: disposeBag)
//next(1)
//next(2)

//만들어진 Sequence에 Error를 일으킬 수 있다.
//Completed되기 전에 에러가 있으면 그 즉시 종료된다.
enum Error: Error {
    case anError
}

Observable<Int>.create { observer -> Disposable in
    observer.onNext(1)
    observer.onError(Error.anError)
    observer.onCompleted()
    observer.onNext(2)
    return Disposables.create()
}
.subscribe(
    onNext: {
        print($0)
    },
    onError: {
        print($0.localizedDescription)
    },
    onCompleted: {
        print("completed")
    },
    onDisposed: {
        print("disposed")
    }
)
.disposed(by: disposeBag)
//1
//The operation couldn’t be completed. (__lldb_expr_42.Error error 0.)
//disposed
```