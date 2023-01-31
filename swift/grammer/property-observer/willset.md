---
layout: page
menubar: property_observer_menu
title: Willset
subtitle: 바뀌기 전에 호출되는 Property Observer
show_sidebar: false
---

멤버변수에 값이 저장되기 바로 직전에 호출되어 "어떤 값이 저장될지"알려줍니다.

```swift
// song.title = "늑대가 나타났다."
struct SongData {
    var title: String = "비어있음" {
        willSet {
            print("기존 : \(title), 바뀔 값 : \(newValue)")
        }
    }
}

// 기존 : 비어있음, 바뀔 값 : 늑대가 나타났다
```

만약 새로 들어온 값(newValue)이 특정 음악일 경우 따로 처리할 수 있겠네요.

제목 색깔을 바꾸거나 굵게 말이죠.