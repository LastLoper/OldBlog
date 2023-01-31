---
layout: page
menubar: table_view_menu
title: Table View
subtitle: iOS의 기본 리스트 뷰
show_sidebar: false
---

모바일 앱에서 다량의 데이터를 직관적이고 간단하게 보여주는 대표적인 방법이 바로 리스트입니다. iOS앱에서는 이 리스트를 테이블 뷰 또는 컬렉션뷰라는 이름으로 제공하고 있는데요, 첫번째 테이블 뷰를 다뤄볼까 합니다. 테이블뷰는 기본적으로 셀(Cell) 타입 4종류를 지원하고, 커스터마이징한 셀로도 만들 수 있습니다.

각설하고 바로 만들어 볼까요?

<br/>

## 스토리보드는 이렇게

### 테이블 뷰

테이블 뷰를 구현하는 방식은 2가지가 있어요.
- TableViewController를 스토리보드에 배치하는 것
- ViewController안에 TableView를 배치하는 것

첫 번째 방법은 TableViewController를 스토리보드에 배치하고 UITableViewController를 상속받은 파일을 연결하면, 구현에 필요한 위임이나 메소드를 Xcode에서 자동으로 코드를 만들어줍니다. 그래서 구현이 한결 편하죠. 하지만 컨트롤러 전체가 테이블 뷰를 위해 만들어 져 있기 때문에 다른 UI를 배치할 수 없다는 단점이 있습니다.

두 번째 방법은 ViewController안에 UITableView를 배치하는 것입니다. 모든 로직을 직접 구현해야 하는 번거로움이 있지만 다른 UIView들과 조금 더 유연하게 화면을 만들 수 있다는 장점이 있어서 개인적으로 선호하는 더 선호하는 편이에요.

물론, 상황에 맞게 구현하면 좋겠죠?

아래와 같이 테이블 뷰를 스토리보드에 배치해주세요.

![ViewController안에 TableView를 배치한 이미지](/img/2022-08-30/tableview_layout.png){: width="80%" }

_ViewController안에 TableView를 배치_

### 테이블 뷰의 셀

- 기본 셀 타입 4종류
기본적으로 Xcode에서 제공하는 셀 타입들 입니다. 아이폰의 기본 앱들에서 많이 보던 UI죠? 오른쪽 Attributes Inspector-Accessroy에서 지정할 수 있습니다. 하나씩 눌러보면 어떤 모습인지 스토리보드에서 바로 확인할 수 있으니 마음에 드는 걸 선택하시면 될 것 것네요.

![Disclosure Indicator](/img/2022-08-30/tableview_cell_types.png){: width="80%"}

_Xcode에서 기본으로 제공하는 Cell Type_

![Setting The Type](/img/2022-08-30/accessroy_types.png){: width="300px" height="80%" }

_테이블뷰 셀의 Accessary 타입들_

- 커스텀 셀
- 
XIB파일을 따로 만들어 셀을 커스터마이징할 수 있습니다.

먼저 파일을 만들 때 **Also Create XIB file**을 체크해주시고요, TableViewCell을 상속받는 클래스를 만들면 아래와 같이 파일이 만들어지는 것을 볼 수 있어요. 이 XIB에 UILabel, UIButton등의 뷰를 배치해보세요. 나만의 멋진 커스텀 테이블 뷰를 만들 수 있습니다.

![Create Cell of Tableview](/img/2022-08-30/create_cell_of_tableview.png){: width="80%" }

_커스터마이징 셀을 위한 XIB파일_

레이아웃을 정하고 서브뷰들을 배치했다면, XIB 파일에 클래스를 연결해줘야 합니다. Inspector에서 아래와 같이 클래스를 연결해주세요.

![Connect class to the cell](/img/2022-08-30/connect_cell_with_class.png){: width="80%"}

_XIB파일와 클래스파일 연결_

코드에서 이 XIB을 찾기 위해선 셀의 이름이 필요합니다. 역시 Inspector에서 설정할 수 있고요, Identifier를 작성해주세요. 이 Identifier가 XIB의 이름이 되는 것입니다.

![Set identifier of cell](/img/2022-08-30/set_identifier_of_cell.png){: width="80%" }

_XIB파일에 아이디 만들어주기_

<br/>

## 데이터 만들기

디자인 패턴중 MVC패턴을 이용해서 만들어볼 생각이에요.

각각 Model, View, Controller인데요, 프로젝트의 소스 코드 구조를 각각의 역할로 구분하고 그룹지어 유지보수나 가독성을 조금 더 향상할 수 있습니다.

![MVC Structure](/img/2022-08-30/mvc_structure.png){: width="80%"}

_MVC패턴의 파일구조_

- DataModel은 데이터가 저장되는, 개발자가 만드는 임의의 자료형이며,
- DataManager는 DataModel과 ViewContorller사이에서 다리역할을 하며 데이터를 가공하고 컨트롤러에 뿌려줍니다.

<br/>

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

테이블뷰는 Delegate와 DataSource 등 프로토콜을 모두 필수로 위임받아 구현해야 합니다.

하지만 그 전에, 구현하기에 앞서 확장(Extension)이라는 개념에 대해서 살짝 언급해볼까 합니다.

스위프트 공식 홈페이지에 설명된 글을 한번 볼까요?

***Extensions add new functionality to an existing class, structure, enumeration, or protocol type. This includes the ability to extend types for which you don’t have access to the original source code (known as retroactive modeling). Extensions are similar to categories in Objective-C. (Unlike Objective-C categories, Swift extensions don’t have names.)***

***확장 기능은 기존 클래스, 구조, 열거형 또는 프로토콜 유형에 새 기능을 추가하는 것이다. 이 기능은 원본 소스 코드에 액세스할 수 없는 유형을 확장하는 기능이 포함된다. 확장자는 Objective-C의 범주와 비슷하다. (Object-C와는 달리 Swift 확장자는 이름이 없습니다.)***

확장(Extension)을 위와 같이 정의하고 있는데, 간단히 이야기해서 이미 정의되어 있는 구조체 및 클래스등을 필요에 따라 확장해서 쓸 수 있다는 것입니다. 말만 봐서는 어떤 건지 와닿지 않으실 것 같은데요, 이미 만들어져 있는 함수를 개발자가 커스텀할 수 있음을 뜻한다고 생각하시면 편할 것 같아요.

먼저 테이블 뷰의 delegate와 dataSource를 아래와 같이 self로 지정해주세요. 현재 컨트롤러에서 확장하겠다는 의미입니다.

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

아래와 같이 UITableViewDelegate구현에 필요한 함수들이 없다며 오류가 뜨는데요, 테이블 뷰를 확장(Extension)해서 구현해 줄게요.

![Set Delegate Of TableView](/img/2022-08-30/set_delegate_of_tableview.png){: width="80%" }

_프로토콜 위임시 내용을 구현안했을 때_

그리고 확장하기 전에 하나만 더 해줄게요.

위에서 만들어준 XIB 파일을 테이블뷰에 등록해주는 작업이 필요합니다. 아직 테이블 뷰는 개발자가 커스텀해서 만든 셀의 존재를 모르고 있거든요.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    .
    .
    .
    //nimName은 클래스명, forCellReuseIdentifier는 셀의 identifier를 넣어준다.
    tableView.register(UINib(nibName: "TableViewCell", bundle: nil), forCellReuseIdentifier: "TableViewCell")
}
```

### DataManager객체 선언과 Delegate와 DataSource 연결

테이블 뷰에 데이터를 넣어 잘 나오는 지 확인해볼게요.

먼저 컨트롤러는 DataManager로부터 데이터를 가져오기 때문에 객체를 만들어 주세요. 이 객체를 통해 셀에 데이터를 넣어주고 테이블 뷰에 뿌려줄 것입니다.

```swift
class ViewController: UIViewContoller {
    private let dm = DataManager()
    .
    .
    .
}
```

### 데이터 소스(DataSource)

UITableViewDataSource를 확장하게 되면 화면에 테이블 뷰를 어떻게 나타낼지 정할 수 있습니다. 몇개의 셀을 보여줄 것인지, 어떻게 보여줄 것인지 등을 말이죠.

```swift
//MARK: - UITableViewDataSource, 
//셀을 얼마나, 어떻게 보여줘야 하는지를 구현한다.
extension ViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        //나타낼 셀의 갯수
        return dm.datas.count

    //  return dm.getDataCount()
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        //커스텀한 셀을 Identifier로 찾아서 가져와 데이터를 넣어준다.
        //만약 해당 Identifier로 셀을 찾지 못한다면 기본 타입인 TableViewCell()을 Return한다.
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "TableViewCell") as? TableViewCell else { return TableViewCell() }        

        let data = dm.datas[indexPath.item]
        cell.nameLabel.text = data.name
        cell.ageLabel.text = data.age.description + "살"

    //  cell.setUpDataAtViews(data)

        return cell
    }
}
```

### 델리게이트(Delegate)

delegate는 터치 이벤트등의 사용자 액션에 대한 응답을 구현하는 일이에요. 여러 메서드를 제공하지만 대게 셀을 선택했을 때와 옆으로 밀어 삭제하는 메서드를 주로 구현합니다.

```swift
//MARK: - ViewController class
class ViewController { 
    ... 
}

//MARK: - UITableViewDelegate
//셀을 터치하는 등의 이벤트 액션을 구현한다.
extension ViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        guard let cell = tableView.cellForRow(at: indexPath) else { return }
        //TO DO
    }   
}
```

<br/>

## 결과

![Final Result](/img/2022-08-30/final_result.png){: width="50%" }