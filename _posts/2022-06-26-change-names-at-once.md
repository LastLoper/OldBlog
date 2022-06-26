---
author: Walter
title: Class, IBOutlet명까지 변수명 한번에 바꾸는 방법 in Xcode
date: 2022-06-26 22:07:00 +0900
categories: [ios, xcode]
tags: [ios, xcode, tips, swift]     # TAG names should always be lowercase
---

XCode의 `찾기-바꾸기`의 기능은 해당 페이지의 모든 동일한 이름을 바꾸지만, 이는 방법은 변수명 뿐만 아니라 클래스명과 IBOutlet명까지 한번에 바꾸는 방법이다.

아래 이미지와 같이 변수명을 `[우클릭] - [Refactor] - [Rename]` 한다.<br>
Class명이나 IBOutlet명도 동일하게 바꿀 수 있다.

![menu](/post_img/0626/menu.png){: }
_rename 메뉴_

이후 XCode는 패키지의 모든 파일을 검색한다. 그래서 시간이 조금 걸릴 수 있다.

![rename result](/post_img/0626/rename_result.png){: }
_rename 결과_

바꾸려는 모든 같은 이름을 찾은 결과를 보여주면, 이름을 바꾸고 `엔터↵`를 치면 전부 바뀐 것을 확인할 수 있다.