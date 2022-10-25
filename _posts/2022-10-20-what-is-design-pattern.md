---
title: 프로그래밍에 디자인 패턴이 무슨 말이야?
date: 2022-10-20 09:13:00 +0900
categories: [프로그래밍, 개념]
tags: [ios, 프로그래밍, 개념, 디자인패턴]
author: WalterCho
---

MVC, MVVM 등 기업에서 요구하는 스펙들이 있다. 바로 **디자인 패턴**이 그것인데, 알면서도 모를듯한 개념이다.<br>
그 개념에 대해 확실히 할겸 정리했다.

## 디자인 패턴이란?
먼저, 디자인 패턴?? 뭔가 프로그래밍과 그렇게 연관이 있을 것 같지 않은 단어들이다. 사실 따지고보면 iOS앱도 하나의 큰 제품이지 않은가, 소프트웨어라는 제품말이다.<br>
즉, 제품을 만들기 위해 설계도가 필요하다는 말씀! 그리고 디자인 패턴이 이 설계도를 ***어떻게 단순화 할지, 어떻게 재사용할지, 어떻게 유지보수를 쉽게 할 수 있게 할 것인지*** 가이드라인을 제시한다고 생각하면 된다.

여기 자동차를 만들기 위한 설계도가 있다.<br>
이 한장의 설계도는 그 어떤 디자인 패턴도 않고 

![한장의 설계도](/post_img/20221020/car.png)
_발퀄이지만.. 자동차 설계도입니다._

***이때! 클라이언트가 바퀴를 바꾸고 싶다고 한다. Oh my god...***<br>
***열심히 지금 바퀴가 좋다고, 어울린다고 설득해보지만 소용없다.***<br>
***이제 새로운 바퀴를 그려야 한다.***<br>
***그럼 이 설계도에서 바퀴 부분만 찢어야 할까?***<br>
***아니면 위에 새로운 종이로 덮어야 할까?***

만약 각 부분을 각각의 종이에 그렸다면 클라이언트의 당황스러운 요청에도 1도 당황하지 않고 바꿀 수 있었을 것이다.<br>
이것이 디자인 패턴을 적용해서 코딩해야 하는 이유다.<br>

즉 디자인 패턴은,<br>
**프로그램의 구조를 단순화하고 유지보수를 쉽게 하기 위한 하나의 전략**이다.

### 장점
- 변화하는 요구에 빠른 대응이 가능하다.
- 각각을 모듈화해서 재사용성이 증가한다.
- 유지보수가 편하다.
- 소스 코드가 단순해지고 보기 쉬워진다.

### 단점
- 객체지향 프로그래밍 기반이기 때문에 설계시 고려해야 한다.
- 프로그램의 전체적인 그림을 그리는데 돈과 시간이 많이 소요된다.(비용 증가)

## 예시
보편적으로 많이 쓰이는 **디자인 패턴 MVC를 적용하기 전 코드와 적용한 후**의 코드를 비교해보면 조금 더 명확하다.<br>
iOS앱에서 TableView구현을 예로 보자

### MVC 디자인 패턴 적용하기 전
TableVC.swift
```swift
class TableVC: UITableViewController {
    //과일 데이터
    var fruits = ["Apple", "Banana", "Grape", "Mango", "Strawberry", "blueberry"]

    override func viewDidLoad() {
        super.viewDidLoad()
        .
        .
        .
        tableView.register(FruitListCell.self, forCellReuseIdentifier: "FruitListCell")
    }
}

//TableViewDataSource
extension TableVC {
    .
    .
    .
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "FruitListCell") as! FruitListCell
        
        let fruit = fruits[indexPath.row]
        cell.fruitNameLabel.text = fruit        //컨트롤러에서 직접 Cell의 멤버변수에 접근한다.
        
        return cell
    }
}
```

위 코드를 보면 데이터 또는 cell의 변경사항이 발생했을 경우, Controller의 코드를 수정해야 한다.<br>
예시 코드는 TableView 구현중에서도 일부분만 가져와서 "까짓 바꾸면 되지"할 수도 있지만, 만약 코드가 몇백 몇천줄이 되면 감히 얼마나 많은 수고가 있을지 상상도 되지 않는다.

반면 적용 후 예시에서는 각각의 부분만 수정해주면 되는 것을 알 수 있으며, 각각의 부분이 컨트롤러와 나뉘어 있어 독립적으로 실행된다. 이는 모듈화로써 재사용성이 증가한다는 것이고 단순해졌다는 것을 의미하기도 한다.

### MVC 디자인 패턴 적용 후
Fruit.swift(Model)
```swift
struct FruitModel {
    //데이터 변경 요청시 이 파일만 수정하면 된다.
    private var fruits = ["Apple", "Banana", "Grape", "Mango", "Strawberry", "blueberry"]

    func getFruitName(idx: Int) -> String {
        return fruits[idx]
    }
}
```

TableViewItem.swift(View)
```swift
struct TableViewItem: UITableViewCell {
    @IBOutlet weak var fruitName: UILabel

    func setUpFruitName(_ fruit: Fruit) {
        self.fruitName = fruit.name
    }
}
```

TableVC.swift(Controller)
```swift
class TableVC: UITableViewController {

    let fruitModel = FruitModel()

    override func viewDidLoad() {
        super.viewDidLoad()
        .
        .
        .
        tableView.register(FruitListCell.self, forCellReuseIdentifier: "FruitListCell")
    }
}

//TableViewDataSource
extension TableVC {
    .
    .
    .
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "FruitListCell") as! FruitListCell
        
        let fruit = fruitModel.getFruitName(idx: indexPath.row)
        cell.setUpFruitName(fruit)        //데이터만 넘겨줄 뿐 cell에 직접적으로 관여하지 않는다.
        
        return cell
    }
}
```

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
회신 후, 최대한 빠른 시간 내에 회신드릴게요! 감사합니다.