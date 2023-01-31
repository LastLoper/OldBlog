---
layout: post
menubar: network_menu
date: 2022-10-18 10:23:00 +0900
title: Swift, URL에 http:// 자동으로 채워넣는 방법
subtitle:  그러니까...
categories: network
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

iOS의 Webkit으로 웹페이지를 띄울 때 URL을 전달한다.

이때 이 URL은 "http://..." 또는 "https://..."로 시작되지 않으면 정상적으로 웹페이지가 뜨지 않는다.

우린 PC에서도 그렇지만 주소입력바 등의 UI에 늘 하던대로 'naver.com' 입력하곤 한다.

이 방법은 iOS에서도 그렇게 입력되는 것을 방지한다.

함수로 만들어 사용할 수도 있고,

```swift
let urlWithHttp:String = addHTMLAuto(urlStr)

//'http://' 자동 삽입
func addHTMLAuto(_ url:String) -> String {
    var strUrl = ""
    var flag = false
    if strUrl.hasPrefix("http://") || strUrl.hasPrefix("https://") {
        flag = true
    }

    if (!flag) {
        strUrl = "http://" + url
    }
        
    return strUrl
}
``` 

String을 확장(Extension)해서 사용할 수도 있다.

```swift
let urlWithHttp:String = urlStr.addHTMLAuto()

//'http://' 자동 삽입
func addHTMLAuto() -> String {
    var strUrl = ""
    var flag = false
    if self.hasPrefix("http://") || self.hasPrefix("https://") {
        flag = true
    }

    if (!flag) {
        strUrl = "http://" + self
    }
        
    return strUrl
}
```