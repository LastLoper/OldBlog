---
title: iOS앱 리스트 만드는 방법, TableView
date: 2022-06-18 14:46:00 +0900
categories: [ios, guide]
tags: [ios, guide]     # TAG names should always be lowercase
author: Walter
---

다량의 데이터를 직관적이고 간단하게 보여주는 방법이 바로 리스트다. iOS앱에서는 이 리스트를 테이블 뷰라는 이름으로 지원한다. 기본적으로 모든 리스트의 아이템인 셀(Cell)은 Xcode에서 4가지 타입을 기본으로 제공하며, 커스터마이징한 셀로도 만들 수 있다.

## 스토리보드는 이렇게
### 테이블 뷰
테이블 뷰를 구현하는 방식은 2가지가 있다.
- TableViewController를 스토리보드에 배치하는 것
- ViewController안에 TableView를 배치하는 것

**TableViewController를 스토리보드에 배치하고 UITableViewController를 상속받은 파일을 연결하면**, 구현에 필수적인 위임이나 메소드를 Xcode에서 만들어 준다. 그래서 구현이 한결 편하다, 하지만 테이블 뷰의 크기를 조절할 수 없다는 문제가 있어 다른 뷰를 배치할 수 없다.

반면, **ViewController안에 TableView를 배치하면**, 모든 로직을 직접 구현해야 하는 번거로움이 있지만 다른 뷰를 같이 배치할 수 있어 레이아웃면에서 조금 더 유연하다.

고로 난 아래와 같이 두 번째 방법으로 많이 만드는 편이다. 물론, 필요에 따라 선택하면 된다.

![ViewController안에 TableView를 배치한 이미지](/post_img/20220830/tableview_layout.png){: }

### 테이블 뷰의 셀
- Xcode에서 제공하는 셀 타입 4가지
Xcode에서는 테이블 뷰의 셀 타입 4가지를 제공한다.<br>(처음에는 이 Accessory를 만들기 위해 이미지뷰를 직접 넣어서 만들고는 했다.)
![Disclosure Indicator](/post_img/20220830/tableview_cell_types.png){: }
_Xcode에서 기본으로 제공하는 Cell Type_

오른쪽 Attributes Inspector-Accessroy에서 지정할 수 있다.<br>
![Setting The Type](/post_img/20220830/accessroy_types.png){: width="300px" height="300px" }

- 커스텀 셀
Xcode에서 4가지 셀 디자인을 제공하긴 하지만, 그래도 내가 원하는 디자인이 있을때는 XIB파일을 따로 만들어서 커스터마이징할 수 있다.

먼저 XIB와 


## 데이터는?


## 코딩
### 확장(Extension)
테이블뷰는 Delegate와 DataSource 등 프로토콜을 모두 필수로 위임받아 구현해야 하는데, 구현하기에 앞서 확장(Extension)이라는 개념에 대해서 살짝 언급해볼까 한다.

***Extensions add new functionality to an existing class, structure, enumeration, or protocol type. This includes the ability to extend types for which you don’t have access to the original source code (known as retroactive modeling). Extensions are similar to categories in Objective-C. (Unlike Objective-C categories, Swift extensions don’t have names.)***

***확장 기능은 기존 클래스, 구조, 열거형 또는 프로토콜 유형에 새 기능을 추가하는 것이다. 이 기능은 원본 소스 코드에 액세스할 수 없는 유형을 확장하는 기능이 포함된다. 확장자는 Objective-C의 범주와 비슷하다. (Object-C와는 달리 Swift 확장자는 이름이 없습니다.)***

Swift 공식문서에서는 확장(Extension)기능을 위와 같이 정의하고 있다.<br>간단히 이야기해서 이미 정의되어 있는 구조체 및 클래스등을 필요에 따라 확장해서 쓸 수 있다는 것이다.


### 델리게이트(Delegate)
아래와 같이 클래스 밑에 정의할 수 있다.
```swift
//MARK: - UITableViewDelegate
extension ViewController: UITableViewDelegate {
    
}
```

### 데이터 소스(DataSource)
```swift

```