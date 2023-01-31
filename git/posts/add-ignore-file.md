---
layout: post
menubar: git_menu
date: 2022-11-21 23:00:00 +0900
title: iOS .gitignore사용법
subtitle:  Git연동시 Pods파일등 불필요한 파일을 제외해서 올려보자,
categories: network
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

프로젝트를 협업할 때 Git 소스 관리 툴을 많이 사용하죠, 그때 _DS_Store등 불필요한 파일까지 올릴 이유는 없을거에요. **.gitignore**파일은 그런 항목들을 적어서 commit할때 배제하도록 도와줍니다. 하지만 매번 일일히 적어주기가 불편하죠. 

.gitignore파일 작성을 도와주는 사이트를 소개합니다.

<br/>

## .gitignore.io

아래 사이트를 참고하면 제외할 항목들을 한번에 정리해서 만들 수 있어요.

[.gitignore 만들어주는 사이트 바로 가기](https://www.toptal.com/developers/gitignore)

![gitignore site](/img/2022-11-21/gitignore_site.png){: width="80%"}

_ginignore.io_

<br/>

업로드에 제외할 항목 키워드들을 입력하고 `생성버튼`을 누르면 아래와 같이 .gitignore에 작성할 내용을 볼 수 있습니다.

![gitingore_contents](/img/2022-11-21/gitingore_contents.png){: width="80%"}

_cocodapods, swift, xcode에 관한 불필요한 내용이 작성된 .gitignore_

내용을 복사해주세요.

<br/>

## .gitignore파일 만들기

터미널을 열어 github에 올릴 프로젝트로 이동하고 아래와 같이 파일을 만들어 주세요.

```terminal
> vim ./.gitignore
```

gitignore 파일은 숨김파일로 만들어져야 하니 앞에 꼭 점(.)을 붙이는 걸 기억해주세요.

이제 복사한 내용을 .gitignore파일에 붙여넣어 주세요.

참, 여기에 .DS_Store도 같이 작성해서 넣어주시면 팀원과 보다 원할하게 코드를 공유할 수 있습니다.