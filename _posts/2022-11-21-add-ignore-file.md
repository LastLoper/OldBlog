---
title: iOS, Git연동시 Pods파일등 불필요한 파일을 제외해서 올려보자, .gitignore사용법
date: 2022-11-21 23:00:00 +0900
categories: [iOS, Tip]
tags: [ios, tip, git, gitignore, pods제외]
author: WalterCho
---

앱 개발등 프로젝트를 진행하다보면 git을 사용할텐데요, 여러 컴퓨터 혹은 다른 팀원과 파일을 공유하다보면 몇몇 파일은 굳이 올리지 않아도 되지요. 그때 사용할 수 있는 방법이 바로 .gitignore파일의 존재입니다. 그런데 이 파일을 어떻게 작성해야 할까요? 아주 쉽게, 보다 효율적으로 만드는 방법을 소개할게요.

## 문서를 만들어주는 사이트
방법은 아주 간단합니다.<br>
아래 사이트를 참고하면 제외할 항목들을 한번에 정리해서 만들 수 있어요.<br>
<https://www.toptal.com/developers/gitignore>
![gitignore site](/post_img/20221121/gitignore_site.png)

이렇게 업로드에 제외할 항목 키워드들을 입력하고 `생성버튼`을 누르면 아래와 같이 .gitignore에 작성할 내용을 볼 수 있습니다.

![gitingore_contents](/post_img/20221121/gitingore_contents.png)

내용을 복사해주세요.

## .gitignore파일 만들기
터미널을 열어 github에 올릴 프로젝트로 이동하고 아래와 같이 파일을 만들어 주세요.
```terminal
> vim ./.gitignore
```

gitignore 파일은 숨김파일로 만들어져야 하니 앞에 꼭 점(.)을 붙이는 걸 기억해주세요.<br>
이제 복사한 내용을 .gitignore파일에 붙여넣어 주세요.

참, 여기에 .DS_Store도 같이 작성해서 넣어주시면 팀원과 보다 원할하게 코드를 공유할 수 있습니다.

## .gitignore파일 업로드
파일을 만들었다면 github로 올려줘야 겠죠?<br>
아래와 같이 git 명령어를 입력해주세요.

```terminal
> git add ./.gitignore
> git commit -m "uploaded .gitignore"
> git push
```

## 메일보내기
포스팅에 잘못된 부분이 있거나 궁금하신 점이 있다면, 왼쪽 사이드 하단 메뉴에서 로퍼즈팀으로 메일을 보내주세요!<br>
최대한 빠른 시간 내에 회신드릴게요! 감사합니다.