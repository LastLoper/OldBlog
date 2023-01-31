---
layout: post
menubar: libraries_menu
date: 2022-12-9 16:16:00 +0900
title: iOS앱 오토레이아웃 스냅킷 Inset, OffSet 어떤 차이?
subtitle: inset은 절대값, offset은 상대값?!
categories: property-observer
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

모든 레이아웃을 스토리보드로 잡을 수 있다면 참 좋겠지만 코드로 뷰를 만들고 레이아웃을 잡아줘야하는 경우가 많다. 이때 유용한 라이브러리가 바로 SnapKit인데, 여백을 띄우는 inset과 offset의 차이가 헷갈려 간단한 실험을 진행했다.

<br/>

## 실험시작

일단 상하좌우 30씩 떨어트린 뷰를 그렸다.

보이진 않지만 `backView`라는 `view`안에 작은 파란색으로 칠 한 `blueView`를 넣은 모습이다.

![screen shot](/img/2022-12-09/screenshot_1.png){: width="30%" }

_상하좌우 30씩 여백이 있는 뷰_

top, trailing, bottom, leading 모두 30씩 띄웠고 offset과 inset으로 이 레이아웃이 어떻게 변하는지 테스트했다.

<br/>

### offset과 inset의 차이

일단은 위 레이아웃을 만들기 위해서 offset과 inset의 코드는 다음과 같다.

```swift
//offset을 이용한 간격 띄우기
blueView.snp.makeConstraints {
    $0.leading.equalToSuperview().offset(30)
    $0.top.equalToSuperview().offset(30)
    $0.trailing.equalToSuperview().offset(-30)
    $0.bottom.equalToSuperview().offset(-30)
}
```

offset의 코드를 보면 trailng과 bottom은 -(마이너스)로 값을 줬는데,

offset이 정해지는 공식이 `현재 뷰의 constraint = 슈퍼뷰의 constraint + offset의 값`이기 때문이다.

그러니까 정리하자면 각 constraint가 결정되는 것은 아래와 같다.

- top의 간격 : 슈퍼뷰의 top + offset(30)
- trailing의 간격 : 슈퍼뷰의 trailing - offset(30)
- bottom의 간격 : 슈퍼뷰의 bottom - offset(30)
- leading의 간격 : 슈퍼뷰의 leading + offset(30)

<br/>

이번에는 inset의 코드다.

```swift
//inset을 이용한 간격 띄우기
blueView.snp.makeConstraints {
    $0.edges.equalToSuperview().inset(30)
}
```

inset은 한 줄로 위 레이아웃이 가능하다. 하지만 offset과 동일한 결과를 얻을 수 있었다.

즉, offset의 공식과는 다르게 적용된다는 것을 뜻한다.

<br/>

그럼 offset도 혹시 한 줄로 될까?

공식에 대로라면.. 아마 오른쪽과 아래쪽은 벗어나겠지만 한번 해봤다.

```swift
//offset을 이용해 4방향 간격 띄우기
blueView.snp.makeConstraints {
    $0.edges.equalToSuperview().offset(30)
}
```

![screen shot](/img/2022-12-09/screenshot_2.png){: width="30%" }

_offset으로 일괄 4방향 간격띄우기 결과_

역시 벗어난다.

<br/>

이번엔 inset에 4방향을 각각 지정하되, 우하에 -(마이너스)의 값을 줬다.

```swift
blueView.snp.makeConstraints {
    $0.leading.equalToSuperview().inset(30)
    $0.top.equalToSuperview().inset(30)
    $0.trailing.equalToSuperview().inset(-30)
    $0.bottom.equalToSuperview().inset(-30)
}
```

![screen shot](/img/2022-12-09/screenshot_2.png){: width="30%" }

_inset으로 각 방향 간격 띄우기 결과_

offset과는 달리 -(마이너스)값을 줫음에도 벗어나는 것을 볼 수 있다.

<br/>

반대로 좌상 constriant에 -(마이너스)값을 주어 확인해봤다.

```swift
blueView.snp.makeConstraints {
    $0.leading.equalToSuperview().inset(-30)
    $0.top.equalToSuperview().inset(-30)
    $0.trailing.equalToSuperview().inset(30)
    $0.bottom.equalToSuperview().inset(30)
}
```

![screen shot](/img/2022-12-09/screenshot_3.png){: width="30%" }

_inset으로 각 방향 간격 띄우기 결과2_

이번엔 -(마이너스)값을 준 leading, top이 `superView`를 벗어났다.

<br/>

## 결론

**offset**은 `superView`를 기준으로 위치가 정해지고,

**inset**은 해당 뷰를 기준으로 절대값이 적용되는 것으로 짐작할 수 있을 것 같다.

<br/>

## 참고 

inset으로 각 방향에 대해 아래와 같이 지정해줄 수 있다.

```swift
blueView.snp.makeConstraints {
    $0.edges.equalToSuperview().inset(UIEdgeInsets(top: 30, left: 30, bottom: 30, right: 30))
//    $0.edges.equalTo(UIEdgeInsets(top: 30, left: 30, bottom: 30, right: 30))
}
```