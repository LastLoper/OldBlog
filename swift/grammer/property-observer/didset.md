---
layout: page
menubar: property_observer_menu
title: Didset
subtitle: 바뀌고 난 후 호출되는 Property Observer
show_sidebar: false
---

역시 멤버변수에 값이 저장되기 전에 호출되지만 "현재 값 이전에 어떤 값이 저장되어 있었는지"를 알려줘요.

```swift
//song.title = "Sweet Dreams"
struct SongData {
    var title: String = "비어있음" {
        didSet {
            print("현재 : \(title), 이전의 값 : \(oldValue)")
        }
    }
}

// 현재 : Sweet Dreams, 이전의 값 : 비어있음
```

보통 저는 테이블뷰 또는 컬렉션뷰등을 새로고침할 때 주로 didSet을 쓰는데요,

역시 willSet과 비슷하게 특정 값에 따라 분기처리할 수 있을 것 같습니다.