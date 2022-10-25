---
title: iOS앱 리스트 만드는 방법, TableView
date: 2022-06-18 14:46:00 +0900
categories: [iOS, Guide]
tags: [ios, guide]     # TAG names should always be lowercase
author: WalterCho
---

다량의 데이터를 직관적이고 간단하게 보여주는 방법이 바로 리스트다. iOS앱에서는 이 리스트를 테이블 뷰라는 이름으로 지원한다. 기본적으로 모든 리스트의 아이템인 셀(Cell)은 Xcode에서 4가지 타입을 기본으로 제공하며, 커스터마이징한 셀로도 만들 수 있다.

## 스토리보드는 이렇게
### 테이블 뷰
테이블 뷰를 구현하는 방식은 2가지가 있다.
- TableViewController를 스토리보드에 배치하는 것
- ViewController안에 TableView를 배치하는 것

**TableViewController를 스토리보드에 배치하고 UITableViewController를 상속받은 파일을 연결하면**, 구현에 필수적인 위임이나 메소드를 Xcode에서 만들어 준다. 그래서 구현이 한결 편하다, 하지만 테이블 뷰의 크기를 조절할 수 없다는 문제가 있어 다른 뷰를 배치할 수 없다.

반면, **ViewController안에 TableView를 배치하면**, 모든 로직을 직접 구현해야 하는 번거로움이 있지만 다른 뷰를 같이 배치할 수 있어 레이아웃면에서 조금 더 유연하다.

고로 난 아래와 같이 두 번째 방법으로 많이 만드는 편이다. 물론, 필요에 따라 선택하면 된다.<br>
![ViewController안에 TableView를 배치한 이미지](/post_img/20220830/tableview_layout.png){: width="450" height="450" }
_ViewController안에 TableView를 배치_

### 테이블 뷰의 셀
- Xcode에서 제공하는 셀 타입 4가지
Xcode에서는 테이블 뷰의 셀 타입 4가지를 제공한다.<br>(처음에 이 Accessory를 만들기 위해 이미지뷰를 직접 넣어서 만들고는 했다.)
![Disclosure Indicator](/post_img/20220830/tableview_cell_types.png){: }
_Xcode에서 기본으로 제공하는 Cell Type_

오른쪽 Attributes Inspector-Accessroy에서 지정할 수 있다.<br>
![Setting The Type](/post_img/20220830/accessroy_types.png){: width="300px" height="300px" }
_테이블뷰 셀의 Accessary 타입들_

- 커스텀 셀
Xcode에서 4가지 셀 디자인을 제공하긴 하지만, 그래도 나만의 셀을 만들고 싶을 때가 있다. 이때 XIB파일을 따로 만들어 셀을 커스터마이징할 수 있다.

먼저 **Also Create XIB file**을 체크해서 TableViewCell을 상속받는 클래스를 만든다. 그럼 아래와 같이 파일이 만들어지는데, 이 XIB에 보여주길 원하는 뷰를 배치하면 커스터마이징 셀이 된다.<br>
![Create Cell of Tableview](/post_img/20220830/create_cell_of_tableview.png){: width="500" height="500" }
_커스터마이징 셀을 위한 XIB파일_

그 전에, 이 XIB 파일에 클래스를 연결해줘야 한다. Inspector에서 같이 만든 클래스를 연결한다.<br>
![Connect class to the cell](/post_img/20220830/connect_cell_with_class.png){: }
_XIB파일와 클래스파일 연결_

하나 더, 이 XIB 파일을 뷰컨트롤러에서도 사용하기 위해서 XIB에 이름을 붙여줘야 한다. 역시 Inspector에서 Identifier를 작성해준다. 이 Identifier가 XIB의 이름이다.<br>이 이름은 아래 UITableViewDataSource { } 에서 사용될 것이다.<br>
![Set identifier of cell](/post_img/20220830/set_identifier_of_cell.png){: width="450" height="450" }
_XIB파일에 아이디 만들어주기_

## 데이터는?
M(Model), V(View), C(Controller) 패턴으로 테이블뷰를 구성하면 아래와 같은 파일 구조를 가진다.
![MVC Structure](/post_img/20220830/mvc_structure.png){: width="450" height="450"}
_MVC패턴의 파일구조_

- DataModel은 데이터가 저장되는 임의의 자료형이며,
- DataManager는 DataModel과 ViewContorller사이에서 데이터를 가공하고 뿌려주는 역할을 한다.

## 코딩
### DataModel과 DataManager
- DataModel
```swift
struct DataModel {
    let age: Int
    let name: String
}
```

- DataManager
```swift
struct DataManager {
    var datas = [
        DataModel(age: 19, name: "JongSu"),
        DataModel(age: 36, name: "YunSeong"),
        DataModel(age: 27, name: "JiWoo")
    ]
    
    func getUserData(idx: Int) -> DataModel {
        return datas[idx]
    }
}
```

### 확장(Extension)
테이블뷰는 Delegate와 DataSource 등 프로토콜을 모두 필수로 위임받아 구현해야 하는데, 구현하기에 앞서 확장(Extension)이라는 개념에 대해서 살짝 언급해볼까 한다.

***Extensions add new functionality to an existing class, structure, enumeration, or protocol type. This includes the ability to extend types for which you don’t have access to the original source code (known as retroactive modeling). Extensions are similar to categories in Objective-C. (Unlike Objective-C categories, Swift extensions don’t have names.)***

***확장 기능은 기존 클래스, 구조, 열거형 또는 프로토콜 유형에 새 기능을 추가하는 것이다. 이 기능은 원본 소스 코드에 액세스할 수 없는 유형을 확장하는 기능이 포함된다. 확장자는 Objective-C의 범주와 비슷하다. (Object-C와는 달리 Swift 확장자는 이름이 없습니다.)***

Swift 공식문서에서는 확장(Extension)기능을 위와 같이 정의하고 있는데,<br>간단히 이야기해서 이미 정의되어 있는 구조체 및 클래스등을 필요에 따라 확장해서 쓸 수 있다는 것이다.

### DataManager객체 선언과 Delegate와 DataSource 연결
보여줄 데이터를 가지고 있는 DataManager객체를 만든다.
```swift
class ViewController: UIViewContoller {
    private let dm = DataManager()
    .
    .
    .
}
```

iOS의 테이블뷰, 컬렉션뷰와 같은 프로토콜을 ViewController 클래스로 아래와 같이 지정한다.<br>
```swift
//MARK: - ViewDidLoad()
class ViewController: UIViewContoller {
    .
    .
    .
    @IBOutlet weak var tableView: UITableView!
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.tableView.delegate = self
        self.tableView.dataSource = self
    }
}
```

그럼 아래와 같이 UITableViewDelegate구현에 필요한 함수들이 없다며 오류가 뜨는 것을 볼 수 있는데, 확장기능(Extension)을 사용해서 구현할 것이다.<br>
![Set Delegate Of TableView](/post_img/20220830/set_delegate_of_tableview.png){: width="450" height="450" }
_프로토콜 위임시 내용을 구현안했을 때_

각 델리게이트를 뷰컨트롤러에 연결했다면, 커스터마이징한 셀. 즉, XIB 파일을 테이블뷰에 등록해주는 작업이 필요하다.<br>
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    .
    .
    .
    tableView.register(UINib(nibName: "TableViewCell", bundle: nil), forCellReuseIdentifier: "TableViewCell")
}
```

### 델리게이트(Delegate)
아래와 같이 클래스 밑에 정의할 수 있다.
```swift
//MARK: - ViewController class
class ViewController { ... }
//MARK: - UITableViewDelegate
//셀 터치 이벤트 등의 액션을 구현한다.
extension ViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        guard let cell = tableView.cellForRow(at: indexPath) else { return }
        //터치한 cell을 확인할 수 있다.
    }   
}
```

### 데이터 소스(DataSource)
아래 코드는 반드시 구현해줘야 하는 함수들로써, 셀을 몇개나, 어떻게 보여줄지를 결정한다.<br>
```swift
//MARK: - UITableViewDataSource, 
//셀을 얼마나, 어떻게 보여줘야 하는지 등을 구현한다.
extension ViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return dm.datas.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "TableViewCell") as? TableViewCell else { return TableViewCell() }
        
        let data = dm.datas[indexPath.row]
        cell.nameLabel.text = data.name
        cell.ageLabel.text = data.age.description + "살"

        return cell
    }
}
```

## 결과
![Final Result](/post_img/20220830/final_result.png){: width="500" height="500" }
_완성_

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
회신 후, 최대한 빠른 시간 내에 회신드릴게요! 감사합니다.