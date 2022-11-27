---
title: Xcode에 파일 추가하는 2가지 방법
date: 2022-11-22 22:41:00 +0900
categories: [iOS, Guide]
tags: [ios, guide, modal, present, style]
author: WalterCho
---

단순히 디렉토리로 파일을 옮기는 것만으로도 바로 표시되고 쓸 수 있도록 하는 IDE가 많습니다, 하지만! 우리의 Xcode에서는 다른 방법으로 파일을 XCode에 넣어줘야 합니다. 저 역시 아주 가끔 디렉토리에만 파일을 넣어놓고 왜 Xcode에는 안나타날까 Build를 몇번씩 하곤 합니다. 이 정도는 만들어 줘도 되는거 아니니 애플아...ㅠ<br>
그래서 준비했습니다, 코드 및 이미지 파일등 XCode로 옮기는 방법!

## 드래그 앤 드랍
iOS앱을 만들어오셨던 분이라면 아주 익숙한 방법인데요, 바로 파일을 XCode로 드래그 앤 드랍하는 것입니다. 이미지 파일을 보통 이렇게 추가하곤 하죠. 코드 소스 파일도 동일하게 옮길 수 있어요.
![drag n drop to move files](/post_img/20221122/drag_n_drap.png)
_드래그 앤 드랍으로 파일을 Xcode로 가져오는 방법_

![copy items window](/post_img/20221122/copy_items.png)

## XCode메뉴 이용하기
두 번째는 Xcode의 메뉴를 이용한 방법인데요, 결과적으로는 드래그 앤 드랍과 동일합니다.<br>
Xcode의 상단의 메뉴바에서 `file` - `add files to "앱이름"`으로 파일을 가져올 수 있어요.
![using_menu_in_xcode](/post_img/20221122/using_menu_in_xcode.png)
_XCode메뉴를 이용해서 파일을 가져오는 방법_

![copty_items2](/post_img/20221122/copty_items2.png)

하지만 전 이 방법을 더 선호하는 편입니다. 왜냐하면 온전히 Xcode내에서만 작업할 수 있기도 하고 단축키를 사용할 수 있기 때문이지요.<br>
***단축키 : 옵션(⌥) + 커맨드(⌘) + a***

## 알아두면 좋아요.
1. Destination을 체크하지 않으면 파일을 지웠을 때 Xcode에서도 해당 파일을 찾을 수 없게되니 반드시 체크해주세요.
2. 파일을 옮긴 후에는 Build 한번 해주시면 좋아요. 왜냐하면 Xcode가 해당 파일의 존재를 알게되어 코드에서 바로 쓸 수 있거든요.

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.