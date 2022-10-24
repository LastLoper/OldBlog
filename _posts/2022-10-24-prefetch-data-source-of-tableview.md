---
title: iOS 네트워킹으로 많은 데이터를 테이블뷰로 페이징하는 방법
date: 2022-10-24 23:09:00 +0900
categories: [iOS, Util]
tags: [ios, util, tableview]
author: WalterCho
---

보통 많은 데이터를 보여주는 방법으로 테이블뷰 또는 컬렉션뷰를 이용하곤 한다.<br>
이번 포스팅에서는 네트워크 통신시 많은 데이터를 보여줄때, 페이지 단위로 보여주고 특정 시점에 다음 페이지에 해당하는 데이터를 재요청하는 방법을 정리했다.

## prefetchDataSource 프로토콜
애플 도큐먼트에서 말하는 prefetchDataSource의 설명을 보면,<br>

***The object that acts as the prefetching data source for the table view,***<br>
***receiving notifications of upcoming cell data requirements.***<br>
***사전 데이터 원본 역할을 하는 개체로, 다가올 셀 데이터 요구 사항에 대한 알림을 받습니다.***

무슨 말인지 썩 한번에 와닿지 않는다..

간단하게 말해서 ***앞으로 불러올 Cell의 index값을 받는***것으로 생각하면 간단하다.<br>
무슨 말이냐면, 현재 6번째 cell을 보여주고 있을 때 아래 메서드의 indexPaths값을 print구문으로 찍어보면 12, 13, 14.. 등의 인덱스값을 가지는 것을 확인할 수 있다.

이제 테이블 뷰를 통한 예시를 보면 조금 더 이해할 수 있을 것이다.

## Usage
테이블 뷰 또는 컬렉션 뷰 모두 사용법은 동일하다.<br>
차이점이 있다면,<br>

테이블 뷰는 **UITableViewDataSourcePrefetching** 프로토콜을 채택하고,<br>
컬렉션 뷰는 **UICollectionViewDataSourcePrefetching** 프로토콜을 채택한다.

먼저 viewDidLoad()에서 프로토콜을 채택한다.

```swift
override func viewDidLoad() {
    ...
    tableView.prefetchDataSource = self
}
```

이 프로토콜을 채택하면 반드시 구현해야 하는 메서드가 있다.<br>
UITableViewDataSourcePrefetching을 확장해서 **prefetchRowsAt** 메서드를 구현하면 된다.

```swift
extension VC: UITableViewDataSourcePrefetching {
    func tableView(_ tableView: UITableView, prefetchRowsAt indexPaths: [IndexPath]) { 
        //TO DO
    }
}
```

TO DO부분에는 앞으로 불러올 인덱스 값(IndexPaths)이 불러올 다음 페이지 번호와 일치하는 지를 확인 후, 일치한다면 해당 페이지 번호를 파라미터로 재요청 하면 된다.

```swift
class VC: UITableViewController {
    var currPage = 1    //데이터를 성공적으로 가져왔을 때 페이지가 증가한다.

    override func viewDidLoad() {
        ...
        tableView.prefetchDataSource = self
    }
}
extension VC: UITableViewDataSourcePrefetching {
    func tableView(_ tableView: UITableView, prefetchRowsAt indexPaths: [IndexPath]) { 
        //첫번째 페이지라면 다시 데이터를 요청할 필요가 없기 때문에
        //2번째 페이지일때부터 작동하도록 한다.
        if self.currentPage != 1 {
            indexPaths.forEach({
                if ($0.row + 1)/<한 페이지에 보여줄 개수> + 1 == self.currPage {
                    self.fetchDatas(of: self.currPage)
                }
            })
        }
    }
}
```

단, 이 방법을 사용하기 위해서는 API에서 page번호에 대한 파라미터를 제공해야 한다.

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
회신 후, 최대한 빠른 시간 내에 회신드릴게요! 감사합니다.