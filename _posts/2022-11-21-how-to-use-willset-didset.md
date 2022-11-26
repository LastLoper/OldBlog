---
title: iOS에서 willSet과 didSet 이렇게 쓸 수 있다.
date: 2022-11-21 21:11:00 +0900
categories: [Swift, Grammer]
tags: [swift, grammer, willset, didset]
author: WalterCho
---

클래스의 멤버변수 즉, 프로퍼티(Property)등에 장착할 수 있는 willSet, didSet은 변수의 상태를 감지하고 응답합니다. 그리고 iOS앱에서 레이블 또는 테이블뷰 및 컬렉션뷰등 값의 변화에 따라 새로고침이 필요한 순간에 아주 유용하게 사용할 수 있지요. 어려운 단어지만 어렵지 않은, 개념과 사용법을 잘 익혀서 보다 짧고 보다 효율적인 앱을 완성해보세요!

## 프로퍼티 옵저버(Property Observer)
애플은 willSet/didSet를 프로퍼티 옵저버라는 이름으로 부릅니다. 프로퍼티 옵저버(Property Observer)?

결론부터 말하자면,<br>
***옵저버(Observer), 말 그대로 클래스의 멤버변수의 변화를 관찰하는 것***입니다.

willSet과 didSet은 getter/setter와 비슷한 느낌입니다.<br>
getter/setter는 개발자가 직접 값을 확인해서 저장할 것인지 말것인지를 결정할 수 있었다면, willSet/didSet은 그냥 저장하되 이제 바뀌는 값, 혹은 전에 저장됐던 값을 확인하고 그에 따라 나머지 작업을 어떻게 할 것인지 결정해라의 차이랄까요?

## 사용법
개념을 살펴봤다면 이제 어떻게 사용할 수 있는지 정리해보죠.
두 프로퍼티 옵저버는 저장 프로퍼티(Stored Property) 즉, 값을 저장하게 되는 멤버변수에 사용할 수 있어요.

하지만, 사용하기 전에!<br>
프로퍼티 옵저버 willSet과 didSet을 쓰기 위한 조건이 있습니다.

***첫번째, 멤버변수가 반드시 초기화 되어 있어야 해요.*** 즉, 옵셔널 변수에는 사용할 수 없다는 것이죠.
이는 내부적으로 감지할 변수의 실체가 있어야 하기 때문이지 않을까 해요. 존재하지도 않는 변수를 감시하고 있을 수 없을테니까요.

이와 비슷한 이유로, ***두번째, init()함수에서 초기화할 때도 willSet/didSet은 호출되지 않아요!***

자, 그럼 willSet과 didSet은 각각 언제 호출되고 어떤 값을 알려줄까요?

### willSet
멤버변수에 값이 저장되기 바로 직전에 호출되어 "어떤 값이 저장될지"알려줍니다.
```swift
// song.title = "늑대가 나타났다."
struct SongData {
    var title: String = "비어있음" {
        willSet {
            print("기존 : \(title), 바뀔 값 : \(newValue)")
        }
    }
}

// 기존 : 비어있음, 바뀔 값 : 늑대가 나타났다
```

만약 새로 들어온 값(newValue)이 특정 음악일 경우 따로 처리할 수 있겠네요.
제목 색깔을 바꾸거나 굵게 말이죠.

### didSet
역시 멤버변수에 값이 저장되기 전에 호출되지만 "현재 값 이전에 어떤 값이 저장되어 있었는지"를 알려줘요.
```swift
struct SongData {
    var title: String = "비어있음" {
        didSet {
            print("현재 : \(title), 이전의 값 : \(oldValue)")
        }
    }
}
```

보통 저는 테이블뷰 또는 컬렉션뷰등을 새로고침할 때 주로 didSet을 쓰는데요,<br>
역시 willSet과 비슷하게 특정 값에 따라 분기처리할 수 있을 것 같습니다.

## iOS에서 쓰기
<!-- 여기에 영상을 한번 넣어봐??? -->
실제로 iOS앱을 만들면서 willSet과 didSet은 이렇게 사용할 수 있어요.<br>

```swift
//게임 타이틀을 표시하는 label 업데이트하기 위해 willSet 사용
class VC: UIViewController {
    @IBOutlet weak var gameTitleLable: UILabel!
    private var gameTitle = "" {
        willSet {
            if checkGameTitle(newValue) {
                //새로 입력받은 타이틀이 진짜 게임 타이틀인지 확인 후,
                //true값을 return한다면,
                //그 때 gameTitleLabel을 바꾸어 줍니다.
                gameTitleLable.text = newValue
            }
        }
    }
}

//테이블뷰를 새로고침하기 위해 didSet 사용
class VC: UIViewController {
    @IBOutlet weak var tableView: UITableView!
    private var game: [GameData] = [] {
        didSet {
            tableView.reloadData()
        }
    }
    
    .
    .
    .

    func fetchData() {
        self.gameData = [
            GameData("엘든링", "PC")
            GameData("젤다의 전설", "닌텐도")
        ]

        //여기서 gameData 값을 바꾸고
        //tableView.reloadData()을 따로 명시 해줄 필요 없습니다.
        //didSet 프로퍼티 옵저버가 값이 바뀐 것을 감지하고
        //tableView.reload()를 수행합니다.
    }
}
```

개인적으로는 willSet/didSet을 쓰는 것과 getter/setter쓰는 것은 비슷한 것 같습니다. 

## 그 외 알아두면 좋아요.
1. 멤버변수에는 willSet, didSet뿐만 아니라 get, set을 연결할 수도 있지만 각각이 하나의 쌍이되요. 그래서 하나의 멤버변수에는 하나만 연결할 수 있어요.
2. 저장 프로퍼티만 프로퍼티 옵저버를 연결할 수 있지만 예외적으로 연산 프로퍼티에도 연결할 수 있어요.