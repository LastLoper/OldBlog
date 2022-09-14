---
title: iOS 스토리보드에서 스크롤뷰 전체를 보고 싶을때
date: 2022-09-13 19:52:00 +0900
categories: [ios, guide]
tags: [ios, guide]     # TAG names should always be lowercase
author: WalterCho
---

SwiftUI가 아닌 Storyboard를 이용한다면, 이렇게 스크롤뷰를 배치할 때가 있다.<br>
그런데 가끔은, 배치하는 뷰들이 Storyboard의 기기 사이즈를 넘을 때도 있다. 바로 이렇게.

![ScrollView Setting but](/post_img/20220913/ScrollView_but.png){: }
_스크롤뷰 셋팅_

이럴 때, 마우스 휠을 움직여 뷰를 위아래로 움직일 수 있다. 하지만, 조금 뿐이다. 그것도 아주 조금.

다행스럽게도 전체 뷰를 Storyboard에서 볼 수 있는 방법이 있다.<br>
그 방법도 아주 간단하다.

## Size Inspector에서 옵션 바꾸기
![Change Simulator Size](/post_img/20220913/SimulatorSize.png){: }
_Size Inspector에서 옵션 바꾸기_

뷰 컨트롤러를 선택 후, Size Inspector에서 Simulated Size를 **Fixed에서 Freeform으로** 바꿔주면된다.<br>
그럼 Storyboard에서 뷰 컨트롤러의 모양이 바뀐 것을 확인할 수 있는데, 아래쪽을 마우스로 잡아끌면 상하 또는 좌우 크기를 조절할 수 있다.

![FullSize Scroll](/post_img/20220913/fullSize.png){: }
_늘어난 뷰 컨트롤러_

이제 다시, It's coding time~~~