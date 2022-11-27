---
title: iOS 목업 데이터? plist로 만들어 쉽고 편리하게 사용하기
date: 2022-11-22 19:44:00 +0900
categories: [iOS, Guide]
tags: [ios, guide, modal, present, style]
author: WalterCho
---

앱을 개발하는 동안 수많은 테스트를 합니다. 그리고 그 테스트를 위해서는 데이터가 필요한데, 이를 목업 데이터라고 하지요. 만약 네트워크 통신으로 데이터를 가져오는 앱이라면 어떻게 할까요? 아직 네트워킹 모듈이 구현되어 있지 않아 어떻게 앱에 나타나는지 알 수 없다면요.<br>
이를 위해 Xcode에서는 목업 데이터를 하나의 파일로 만들어 사용할 수 있도록 하고 있습니다. 바로 plist입니다.<br>
물론 plist는 더 많은 일을 할 수 있지만 오늘은 목업 데이터 용으로 사용하는 방법을 소개해 볼까 합니다.

## plist로 데이터 만들기
그럼 바로 plist를 만들어보겠습니다. 만드는 방법은 어렵지 않아요.<br>
앱에서 PropertyList 파일을 만들어주세요.
![create property list file](/post_img/20221122/create_property_list.png)

Xcode는 데이터를 입력하는 방법으로 Property List와 Source Code라는 에디터 화면을 제공합니다.<br>
왼쪽 프로젝트 리스트에서 어떤 에디터를 사용할지 선택할 수 있어요.
![opne as](/post_img/20221122/open_as.png)

### Property List에서 데이터 추가
Root아래로 Key/Type/Value를 작성하면 됩니다.<br>
하지만!<br>
우리는 목업 데이터의 용도로 plist를 만들 생각이기 때문에 약간의 수정이 필요합니다.<br>
먼저 아래와 같이 Root아래에 array를 만들고 Root의 Type을 Array로, array의 Type을 Dictionary로 바꾸어 주세요.
![create array field](/post_img/20221122/array_in_plist.png)

이제 이 item에 실제 앱에 뿌려질 데이터와 같은 Key와 Type을 작성해주시면 됩니다.<br>
(value까지 미리 넣으면 이따가 수정하기 힘드니 Key와 Type만 작성해주세요.)
![create items](/post_img/20221122/write_key_type.png)

마지막으로 테스트할 갯수만큼 복사 & 붙여넣기 해주세요.<br>
그리고 이제 각각의 value를 입력해주면 됩니다.

### Source Code에서 데이터 추가
Source Code에서 입력하는 방법입니다.<br>
`plist 우클릭`-`open as`-`Source Code`를 선택해서 에디터 화면을 바꿔주세요.

Source Code화면은 XML형태로 제공되기에 각각 키워드를 태그로 써서 열고, 닫아주면 됩니다.<br>
Property List 에디터 화면에서 작성한 내용은 아래와 같이 작성할 수 있습니다.
![open plist as source code](/post_img/20221122/open_plist_as_source_code.png)

이렇게 목업 데이터 용도로 plist를 만들어봤는데요, 어떠셨나요? 간단하죠?<br>
그럼 이제 어떻게 이 값을 가져올 수 있는지 살펴볼게요.

## plist 데이터 가져오기
먼저 코드 부터 보시죠.<br>
그런데.. 딱히 부가적인 설명이 필요할 정도로 어려운 코드가 아니라서 주석을 참고해주시면 될 것 같습니다.
```swift
struct Movies: Decodable {
    //Data형을 Movies형으로 바꾸기 위해 Decodable 프로토콜을 위임받는다.
    //plist에서 만든 Key와 변수의 이름이 동일해야 한다.
    //다른 변수명을 쓰고 싶다면 CodingKeys객체를 이용할 수 있다.
    let title: String
    let releseDate: String
}

private extension ViewController {
    func getPlistData() {
        let fileName = "Movies"
        
        //프로젝트에서 파일을 가져오기 위해서는 Bundle객체를 사용할 수 있다.
        //파라미터로 파일이름과 확장자를 전달한다.
        guard let url = Bundle.main.url(forResource: fileName, withExtension: "plist") else { return }        
            do {
                //파일을 가져오는 작업은 예외처리를 위해 do { } catch { }를 쓴다.
                //Bundle객체로 불러온 파일을 Data형으로 변환한다.
                //Data형으로 변환된 파일을 다시 개발자가 사용하기 쉽게 Movies형으로 변환한다.
                let data = try Data(contentsOf: url)
                let result = try PropertyListDecoder().decode([Movies].self, from: data)
                
                //TODO

            } catch let error {
                print("error: \(error.localizedDescription)")
            }
        }
    }
}
```

어떠신가요? plist로 만든 목업 데이트를 완벽하게 가져오셨나요?<br>
만약 글에 잘못된 점이 있거나 실패하시는 분들은 메일을 보내 주세요~!

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.