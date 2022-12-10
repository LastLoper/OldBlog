---
title: iOS앱 오토레이아웃 스냅킷 Inset, OffSet 이때 쓰세요.
date: 2022-12-9 16:16:00 +0900
categories: [iOS, Guide]
tags: [ios, guide, autolayout, inset, offset]
author: WalterCho
---

모든 레이아웃을 스토리보드로 잡을 수 있다면 참 좋겠지만 코드로 뷰를 만들고 레이아웃을 잡아줘야하는 경우가 있습니다. 이때 유용한 라이브러리가 바로 SnapKit인데요, 쓰면서도 헷갈리는 inset과 offset!! 둘 다 간격을 띄우는 옵션이고 별차이가 없는 것 같은데.. 무슨 차이가 있는 걸까요? 한번 정리해봤습니다!

## 실험시작
먼저 레이아웃 샘플을 하나 볼까요?<br>
![screen shot](/post_img/20221209/screenshot_1.png){: width="300" }
_SnapKit 예시_

큰 `backView`안에 작은 `blueView`를 넣은 모습인데요, top, trailing, bottom, leading 모두 30정도씩 간격을 띄운 예시입니다. offset과 inset으로 어떻게 이 레이아웃을 만들 수 있는지, 어떤 차이가 있는지 살펴볼게요!

먼저 offset을 이용한 코드입니다.
```swift
//offset을 이용한 간격 띄우기
blueView.snp.makeConstraints {
    $0.leading.equalToSuperview().offset(30)
    $0.top.equalToSuperview().offset(30)
    $0.trailing.equalToSuperview().offset(-30)
    $0.bottom.equalToSuperview().offset(-30)
}
```

이번엔 inset을 이용한 코드입니다.
```swift
//inset을 이용한 간격 띄우기
blueView.snp.makeConstraints {
    $0.edges.equalToSuperview().inset(30)
}
```

두 코드 모두 위 샘플의 레이아웃을 구성할 수 있습니다.<br>
offset은 각 방향을 하나씩 설정했구요, inset은 한 줄로 4방향을 일괄적으로 처리했습니다.<br>
뭔가 갸웃하게 되죠? 똑같은 역할이고 왠지 inset이 더 나아보이는데.. 왜 offset과 inset을 나눠놓은 걸까요? 하나씩 살펴보겠습니다.

offset으로도 한 줄로 간결하게 처리해보겠습니다.

```swift
//offset을 이용해 4방향 간격 띄우기
blueView.snp.makeConstraints {
    $0.edges.equalToSuperview().offset(30)
}
```

![screen shot](/post_img/20221209/screenshot_2.png){: width="300" }
_offset으로 일괄 4방향 간격띄우기 결과_

앗! trailing과 bottom이 `backView`를 벗어났네요. 흐음.. 왜 슈퍼뷰를 벗어났을까요?<br>
다시 이 코드를 보면 왜 trailing과 bottom에 마이너스(-)값을 줘야했는지 이해가 되실 것 같은데요,

```swift
//offset으로 4방향 간격 띄우기
blueView.snp.makeConstraints {
    $0.leading.equalToSuperview().offset(30)
    $0.top.equalToSuperview().offset(30)
    $0.trailing.equalToSuperview().offset(-30)
    $0.bottom.equalToSuperview().offset(-30)
}
```

좌상단을 기준으로 `blueView의 constraint = backView(슈퍼뷰)의 constraint + offset 값` 공식이 적용되기 때문입니다.<br>
- top의 간격 : 슈퍼뷰의 top + offset(30)
- trailing의 간격 : 슈퍼뷰의 trailing - offset(30)
- bottom의 간격 : 슈퍼뷰의 bottom - offset(30)
- leading의 간격 : 슈퍼뷰의 leading + offset(30)<br>
 
![offset](/post_img/20221209/offset.png){: width="400"}
_좌상단을 기준으로 offset이 적용되는 예시_

이번엔 inset으로 4방향을 개별적으로 띄워볼까요?

```swift
//inset으로 4방향 간격 띄우기
blueView.snp.makeConstraints {
    $0.leading.equalToSuperview().inset(30)
    $0.top.equalToSuperview().inset(30)
    $0.trailing.equalToSuperview().inset(-30)
    $0.bottom.equalToSuperview().inset(-30)
}
```
![screen shot](/post_img/20221209/screenshot_2.png){: width="300" }
_inset으로 각 방향 간격 띄우기 결과_

어라?? 마이너스(-)값을 준 trailing과 bottom이 슈퍼뷰를 벗어났네요! inset은 offset의 공식과는 다른 것 같습니다.<br>
반대로 leading과 top에 마이너스(-)값을 적용해보겠습니다.

```swift
blueView.snp.makeConstraints {
    $0.leading.equalToSuperview().inset(-30)
    $0.top.equalToSuperview().inset(-30)
    $0.trailing.equalToSuperview().inset(30)
    $0.bottom.equalToSuperview().inset(30)
}
```
![screen shot](/post_img/20221209/screenshot_3.png){: width="300" }
_inset으로 각 방향 간격 띄우기 결과2_

아하, 뭔가 감히 잡히시나요?<br>
만약 샘플과 같은 레이아웃을 각 방향에 inset으로 간격을 띄우고 싶다면 모든 방향에 플러스(+)를 줘야하겠군요.<br>
이는 즉, offset은 좌상단을 기준으로 간격을 띄웠다면, `inset은 절대값으로 간격을 띄운다는 것`을 알 수 있습니다.

## 결론
### offset
각 방향에 대해 개별적으로 간격을 띄우고 싶을 때 사용하면 적합합니다.

### inset
최소 둘 이상의 방향에 대해 간격을 띄우고 싶을 때 사용하면 적합합니다.<br>
참고로 inset도 아래와 같이 각 방향에 대해 개별적으로 간격을 띄울 수 있습니다.
```swift
blueView.snp.makeConstraints {
    $0.edges.equalToSuperview().inset(UIEdgeInsets(top: 30, left: 30, bottom: 30, right: 30))
//    $0.edges.equalTo(UIEdgeInsets(top: 30, left: 30, bottom: 30, right: 30)) 위 코드와 동일
}
```


## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.