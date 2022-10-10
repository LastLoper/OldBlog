---
title: iOS 스크롤뷰 오토레이아웃 한방에 잡는 간단한 방법
date: 2022-09-18 18:19:00 +0900
categories: [iOS, Guide]
tags: [ios, guide]     # TAG names should always be lowercase
author: WalterCho
---

스크롤뷰는 오토레이아웃을 잡을 때마다 헷갈려서 순서를 정리했다.

## 스크롤뷰 오토레이이아웃
역시 가장 먼저 할 일은 뷰컨트롤러에 스크롤뷰를 배치하는 것이다. 그럼 왼쪽 파일구조에서 스크롤뷰 밑으로 `Content Layout Guide`와 `Frame Layout Guide`가 생기는 것을 볼 수 있는데, 사실 이것때문에 매번 헷갈리지 않았나 싶다.
![Setting in place scrollview](/post_img/20220918/setting_in_place_scrollview.png)
_스토리보드에 스크롤뷰 배치하기_

그래서 찾아보니 일반적인 스크롤뷰를 만들때는 굳이 필요없다는 것을 확인했다.<br>
왼쪽 인스펙터에서 `Gontents Layout Guide 체크박스를 해제`하자, 한결 쉬워진다.

![UnCheck Contents Layout Guide](/post_img/20220918/uncheck_contents_layout_guide.png)
_Contents Layout Guide 체크 해제하기_

그럼 체크박스를 해제하면 스크롤뷰의 파일구조가 바뀌는 것을 확인할 수 있는데, 이제 컨텐츠뷰가 될 UIView를 스크롤뷰에 넣어서 본격적으로 오토레이아웃을 잡아보자.

## 컨텐츠뷰 오토레이아웃
### 1. 상하좌우 Spacing은 스크롤뷰와 꼭 맞게
사진과 같이 View의 Top, Trailing, Bottom, Leading 모두 스크롤뷰에 맞춘다. 모든 Spacing이 0임에도 불구하고 View의 크기가 늘어나지 않는 이유는 스크롤뷰의 크기가 아직 정해지지 않았기 때문인데, 이를 위해서 View의 크기가 먼저 확실해져야 한다.
![Insert contentsView of scrollview](/post_img/20220918/insert_contents_view_of_scrollview.png)
_스크롤뷰의 컨텐츠뷰 배치_

Spacing을 0으로 맞추고 늘어나지 않은 View의 크기는 마우스로 직접 드래그해서 넓혀준다.
![Scrollview with unchecked guide](/post_img/20220918/setting_contents_view_auto_layout.png)
_스크롤뷰의 컨텐츠뷰 오토레이아웃_

### 2. 가로길이는 스크롤뷰와 똑같이
View의 가로길이와 세로길이를 이제 정해줘야 하는데, 먼저 가로길이를 스크롤뷰와 같게 정해준다.
![equal width with scrollview](/post_img/20220918/equal_width_with_scrollview.png)

이때 컨텐츠뷰 width값의 Multiplier를 보면 1이 아니다. 그래서 아래와 같이 꽉차게 보이지 않는다.
![No multiplier](/post_img/20220918/width_no_multiplier.png)
_Multiplier가 1이 아닌 상태_

Multiplier를 1로 맞춰서 꽉채워주자.
![Setting width multiplier](/post_img/20220918/width_multiplier.png)
_Multiplier를 1로 설정_

### 3. 늘어날 세로길이의 Priority는 낮게
이제 세로길이만 잡아주면 되는데, 세로길이의 경우 안에 들어가는 컨텐츠 양에 따라 그 높이가 바뀐다. 그래서 스크롤되는 것이다. 이를 위해 높이가 상황에 맞게 유동적으로 바뀔 수 있도록 우선순위를 낮춰주면 된다.

먼저 화면을 꽉채운 View의 높이를 아래와 같이 그대로 잡아준다.
![setup the height](/post_img/20220918/setup_height.png)
_높이 설정_

그리고 높이의 priority값을 250으로 낮춘다.
![setup the height priority](/post_img/20220918/setup_height_priority.png)
_높이 우선순위 조절_

이제 컨텐츠뷰에 버튼이나 이미지뷰등을 배치해서 확인해보면 잘 작동하는 것을 볼 수 있다.

<!-- gif 이미지를... -->

<!--스토리보드에서 뷰를 시뮬레이터 크기 이상으러 넣고 싶다면, 아래 링크에서 사이즈를 조절할 수 있다.
링크 -->
<!-- 링크는 어떻게.. -->

It's coding time~~
