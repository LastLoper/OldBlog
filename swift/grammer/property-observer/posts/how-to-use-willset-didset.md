---
layout: post
menubar: property_observer_menu
date: 2022-11-16 21:11:00 +0900
title: iOS에서 효과적으로 사용하는 willSet, didSet
subtitle:  iOS에서 willSet, didSet 이렇게 사용하면 좋다.
categories: property-observer
description: iOS에서 willSet, didSet 이렇게 사용하면 좋다.
published: true
---

실제로 iOS앱을 만들면서 willSet과 didSet은 이렇게 사용할 수 있어요.

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
            //didSet 프로퍼티 옵저버가 값이 바뀐 것을 감지하고 tableView.reload()를 수행합니다.
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

        //fetchData()에서 gameData 값을 바꾸고
        //tableView.reloadData()을 따로 명시 해줄 필요 없습니다.
    }
}
```

개인적으로는 willSet/didSet을 쓰는 것과 getter/setter쓰는 것은 비슷한 것 같습니다. 다만 그 사용 용도가 조금 다른데요, ***getter/setter가 캡슐화 특성을 살리기 위해, 즉 데이터의 값을 함부로 바꿀 수 없도록*** 하는데 초점이 맞춰져 있는데 반해 ***willSet/didSet은 기존의 값과 비교 혹은 특정 값을 감지하고 뷰를 업데이***트 하는 데 있는 것 같습니다. 하지만 확실한건 iOS앱을 만들면서 willSet/didSet을 잘 사용하면 코드도 간결해고 효율적인 측면도 상승한다고 생각합니다.

<br/>

## 그 외 알아두면 좋아요.

1. 멤버변수에는 willSet, didSet뿐만 아니라 get, set을 연결할 수도 있지만 각각이 하나의 쌍이되요. 그래서 하나의 멤버변수에는 하나만 연결할 수 있어요.
2. 저장 프로퍼티만 프로퍼티 옵저버를 연결할 수 있지만 예외적으로 연산 프로퍼티에도 연결할 수 있어요.