---
title: 깃 기본 명령어
author: kimsangtak
date: 2021-08-24 21:22:00 +0800
categories: [Blogging, Tutorial]
tags: [git]]
math: true
mermaid: true

---
## git 올리는방법
---


git 로그인하기 위해서
git config --global user.name "honggildong"
git config --global user.email honggildong@example.com
* global 옵션은 git의 전역설정

프로젝트를 시작하고 git으로 관리를 하려면 사용하고자하는 로컬의 디렉토리로 가서
git init 명령어를 입력하게 되면 로컬의 디렉토리에 .git 이란 숨겨진폴더가 생성된다

git remote add origin 깃허브 주소
* origin이란 별칭으로 사용하겠다는 뜻
* git remote -v 명령어 시 별칭 입력한 주소 나옴

git add .
git commit -m '커밋내용'
git push origin master


아직 미숙하지만 더욱더 많이 써보고 포스팅을 늘리도록하겠습니다.

