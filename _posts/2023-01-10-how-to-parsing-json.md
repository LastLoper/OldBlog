---
title: JSON 파싱하는 방법
date: 2023-01-10 09:00:00 +0900
categories: [iOS, Guide]
tags: [ios, guide, json, parsing]
author: WalterCho
---

아마 대부분의 데이터를 전달하는 포맷이 JSON이지 않을까 싶습니다.<br>
JavaScript Object Notation, JSON은 주로 서버에서 클라이언트로, 혹은 클라이언트에서 서버로 데이터를 전송할 때 일반적으로 쓰는 포맷이죠?<br>
iOS앱도 네트워크 통신을 하게되면 주로 이 JSON 형태의 데이터를 전달 받게 되는데요, 어떻게 읽을 수 있을까요!?<br>

## JSON Parsing
### 방법 1
Swift4 이전에는 JSONSerialization을 이용해서 JSON 데이터를 아래와 같이 파싱했었습니다.

```swift
// 받는 데이터 형식이 JSON인 경우, model 타입으로 변환
guard let jsonToArray = try? JSONSerialization.jsonObject(with: data) else {
    fatalError("JSON to Arry Error")
    return
}
//jsonToArray객체로 TODO
do {
    let data = try JSONDecoder().decode(CreditCard.self, from: jsonToArray)
}
```

하지만!<br>
이젠 위 작업을 Swift4.0을 발표하면서 Codable을 상속받은 객체로 바로 처리할 수 있게 되었지요!

```swift
//Swift4에서 JSON Serialize 작업이 객체 단위(Codable)로 가능해짐
let decoder = JSONDecoder()
print(response as Any)
do{
    //TODO
    let json = try decoder.decode(HolidayModel.self , from: data)
}
catch{
    print(error.localizedDescription)
}
```

```swift
do {
    let jsonData = try JSONSerialization.data(withJSONObject: doc.data(), options: [])
    let creditCard = try JSONDecoder().decode(CreditCard.self, from: jsonData)
    return creditCard
} catch let error {
    print("error : \(error.localizedDescription)")
    return nil
}
```

### 방법 2



## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.