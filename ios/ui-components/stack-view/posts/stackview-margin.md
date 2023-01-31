---
layout: post
menubar: stack_view_menu
date: 2023-01-08 11:00:00 +0900
title: StackView 안쪽으로 margin? padding? 여백 주는 방법!
subtitle: 그러니까...
categories: property-observer
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

StackView는 AutoLayout을 보다 편하게 구성할 수 있도록 해주는 View라서 복잡한 Layout을 구현하기에 딱 좋은 것 같아요. 그런데 이번에 이 StackView의 안쪽으로 여백을 줘야할 일이 있어 찾아 정리해봤습니다!<br>

## StoryBoard에서 여백주기
스토리보드에서 주는 방법은 비교적 간단합니다.

![](/img/2023-01-08/stackView_no_margin.png)
_안쪽 여백은 없는 StackView_

좌우 16씩 여백을 주었고, StackView의 백그라운드 색상은 회색!<br>
그런데 View와 View 간격인 Spacing 부분외에는 안보이죠?

바깥쪽은 16씩 간격을 띄웠지만, 안쪽에는 여백이 없다는 뜻이에요.

이제 스택뷰를 선택하시고 오른쪽 인스펙터의 메뉴를 보면, `Layout Margins`라는 속성이 있습니다.<br>
이 버튼을 누르고 `Language Directinal`을 선택해주세요!<br>

![](/img/2023-01-08/stackView_margin.png)
_안쪽 여백을 줄 수 있는 Language Directinal 속성_

그럼 인스펙터에서 상하좌우 여백값이 나타나고, StackView에 여백이 뙇! 생기는게 보이시죠?<br>
이 값을 조정해주시면 원하는 부분의 안쪽으로 여백을 줄 수 있습니다!

![](/img/2023-01-08/stackView_edge_margin.png)
_상하좌우 여백이 설정된 StackView_

## 코드로 여백주기
코드 역시 간단해요.<br>
설명에 앞서 먼저 코드부터 볼까요?
```swift
stackView.layoutMargins = UIEdgeInsets(top: .zero, left: 16, bottom: .zero, right: 16)
```

헉! 한 줄이라니!? 너무 간단하죠?<br>
근데 이 여백값들이 반영 되려면 한가지 속성을 설정해줘야 합니다.

```swift
stackView.isLayoutMarginsRelativeArrangement = true
```

`isLayoutMarginsRelativeArrangement` 바로 이녀석인데요,<br>

![](/img/2023-01-08/isLayoutMarginsRelativeArrangement.png)
_Apple Document isLayoutMarginsRelativeArrangement설명_

애플 공식 문서에서 보면 안쪽 뷰들에 여백을 줄 것인지 결정하는 옵션인 것이죠.<br>
즉, 이 속성의 기본 값이 false이기 때문에 StackView의 안쪽에 배치된 뷰에 여백을 줄 수 없는 것입니다.

결국 두 속성이 설정되어야 반영되는 것!
```swift
stackView.layoutMargins = UIEdgeInsets(top: .zero, left: 16, bottom: .zero, right: 16)
stackView.isLayoutMarginsRelativeArrangement = true
```