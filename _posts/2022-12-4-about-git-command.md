---
title: 필요한 Git 명령어 정리
date: 2022-12-4 18:18:00 +0900
categories: [Git, Command]
tags: [git, command]
author: WalterCho
---

## 로컬 저장소에서 사용할 수 있는 명령어들
스테이징(git add 명령어로 업로드된 위치)과 커밋을 한번에 할 수 있다.<br>
단, 한번이라도 커밋한 파일에 대해서만 가능하다.
- git commit -am “message”

커밋 기록 자세히 볼 수 있으며, 추가 옵션으로 다양한 형태로 볼 수 있다.
- git log
  - git log —oneline (한줄로 보기)
  - git log —oneline —branches (브랜치별로 기록 보기)
  - git log —oneline —branches —graph (그래프로 기록 보기)
  - git log —oneline —all —graph(모든 브랜치 그래프로 확인)

현재 작업중인 파일과 git에 있는 최신버전의 파일이 어떻게 다른지 확인할 수 있다.
- git diff

커밋과 관련된 파일까지 함께 보는 방법이다. 
- git log --state

바로 직전 커밋한 메세지를 수정할 수 있다.<br>
vim 에디터가 뜨면 원래 메세지가 상단에 나타나고 이를 수정, 저장한 후 종료하면 메세지가 수정되고 이전 커밋에 더해진다.<br>
단, 가장 마지막 커밋에 대해서만 가능하다.
- git commit --amend

작업 트리에서 수정한 내용을 취소할 수 있다.
- git restore <파일명>

git add로 업로드한 상태 즉, 스테이징 상태에서 add하기 전으로 되돌린다.
- git restore —staged <파일명>

git commit으로 올라간 최신 커밋을 되돌린다.
- git reset HEAD^

git log등을 통해 커밋 해시를 확인하고 특정 커밋으로 되돌린다.
: git reset —hard <복사한 커밋 해시>

이때, 커밋을 취소하더라도 기록은 남겨둘 수 있는 방법이다.
: git revert <복사한 커밋 해시>

## Github와 Local 저장소를 연결하기
원격 저장소와 연결한다.
- git remote add origin <저장소 주소>

연결 상태를 확인한다.
- git remote -v

## 서로 다른 컴퓨터와 협업하기
프로젝트의 HTTPS 주소를 복사 후 터미널에서 git clone명령어로 특정 디렉토리로 코드를 가져온다.
- git clone <원격 repositofy URL> <복사할 directory>

처음으로  push할때는 -u 옵션을 붙여 해당 브랜치로 push할 것을 알린다. 최초 1번만 수행하면 된다.
- git push -u origin <브랜치명>

빠른 병합등으로 코드를 병합하기 전에 원격 브랜치 정보 가져온다.<br>
실제로 git log, git status등으로 확인해도 파일은 확인할 수 없다.
: git fetch

이 명령어는 현재 로컬 저장소에 커밋된 파일과 원격 저장소의 파일과의 차이점을 확인할 수 있다.
: git diff HEAD origin/main

확인 후, 병합한다.
: git merge origin/main

## 그 외
### Markdown 문법
링크 거는 방법이다.
- <주소>
- [대체할 text](주소)
- [대체할 text](주소, “부가 설명”)

“부가 설명”은 마우스 오버시 나타난다.

이미지를 Github에 올리는 방법이다.<br>
[Github] -> [addfiles] -> [이미지 업로드]<br>
현재 위치를 기준으로 한다.
- ![이미지](./<이미지 파일명>)