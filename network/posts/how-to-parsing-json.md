---
layout: post
menubar: network_menu
date: 2023-01-11 16:13:00 +0900
title: JSON데이터 포맷, iOS에서 파싱하는 방법
subtitle:  그러니까...
categories: network
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

아마 대부분의 데이터를 전달하는 포맷이 JSON이지 않을까 싶습니다.

JavaScript Object Notation, JSON은 주로 서버에서 클라이언트로, 클라이언트에서 서버로 데이터를 전송할 때 사용하는 일반적인 포맷입니다.

iOS앱도 네트워크 통신을 하게되면 주로 이 JSON 형태의 데이터를 전달 받게 되는데요, 어떻게 읽을 수 있을까요!?

<br/>

## JSON Parsing

### JSONSerialization

Swift4 이전에는 JSONSerialization을 이용해서 JSON포맷의 데이터를 아래와 같이 파싱했었습니다.

```swift
// 받는 데이터 형식이 JSON인 경우, model 타입으로 변환
guard let jsonToArray = try? JSONSerialization.jsonObject(with: data) else {
    fatalError("JSON to Arry Error")
    return
}
//jsonToArray객체로 TODO

```

하지만 이젠 위 작업을 Swift4.0을 발표하면서 Codable을 상속받은 객체로 바로 처리할 수 있게 되었지요!

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

### JSON
