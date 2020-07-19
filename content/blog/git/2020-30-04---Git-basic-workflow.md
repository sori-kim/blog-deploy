---
title: "[Git] Git 작업흐름과 기본명령어"
date: "2020-04-30T00:12:03.284Z"
template: "post"
draft: false
slug: "git/200430/1"
category: "git"
tags:
  - "git"

description: " Git은 어떤 프로젝트를 진행할때 ‘시간 여행이 가능한 평행 우주를 만드는것’과 같다. 메인작업을 진행하면서 실험적인 작업을 해보고 싶을때, 깃이 없다면 폴더를 통째로 복사해서 각각에서 진행을 해야겠지만 깃에서는 폴더 안에서 여러 평행우주를 생성해서 그 우주들 안에서 각각 다른 버전으로 프로젝트를 관리할 수 있다."
socialImage: "/media/42-line-bible.jpg"
---

<!-- - [The first transition](#the-first-transition)
- [The digital age](#the-digital-age)
- [Loss of humanity through transitions](#loss-of-humanity-through-transitions)
- [Chasing perfection](#chasing-perfection) -->

# 1. Git을 사용해야 하는 이유

Git은 어떤 프로젝트를 진행할때 **‘시간 여행이 가능한 평행 우주를 만드는것’** 과 같다. 메인작업을 진행하면서 실험적인 작업을 해보고 싶을때, 깃이 없다면 폴더를 통째로 복사해서 각각에서 진행을 해야겠지만 깃에서는 폴더 안에서 여러 평행우주를 생성해서 그 우주들 안에서 각각 다른 버전으로 프로젝트를 관리할 수 있다. 혹은 개발을 하다가 어떤 단계로 되돌아가고 싶을때 단순히 ctrl + Z 차원이 아니라 아예 그때 그 시간으로 되돌아가서 작업을 할 수 있다.이렇게 시공간을 넘나들 수 있는 🐶쩌는 능력을 사용하지 않을 이유가 없음!

=> 이 평행우주를 어떻게 만들고 사용할까? 깃을 생성하고 현재 시점에서 타임캡슐(커밋)에 묻어놓으면 된다.

<br>

# 2.Git Basic Work Flow

## Git basis

Git을 사용해서 파일 버전 관리를 할때 파일은 다음 3개의 상태중 하나의 상태에 있게 된다.

![](https://images.velog.io/images/rimu/post/4af4be68-2648-47d5-9f5f-4b77edd13313/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-04-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.13.22.png)

`Modified`
Modified file은 이름 그대로 수정된 file이다. 하지만 아직 "commited" 되지 않은 상태의 file을 말한다.

`Staged`
Staged file은 modified file에서 한단계 더 나아가서 곧 commit 될거라고 mark 해놓은 상태이다. 즉 modified 와 committed의 중간 상태라고 할 수 있다.
이렇게 중간 상태가 존재 하는 이유는, commit 하기전에 중간 상태를 저장할 수 있도록 하기 위해서다. Commit을 하면 commit history에 남기도 하고, 혹시 추가 수정 사항이 있거나 다시 되돌려야 할때 까다롭기 때문에 (commit 후에도 다시 되돌리는게 가능은 함) commit 전에 중간 상태에 저장할 수 있도록 하는 것이다. 즉, commit은 해당 개발이 완전 완료 됬을때 하는 것이기 때문에, 아직 완료는 안되었지만 그래도 중간 상태를 저장할 필요가 있을때 staging을 사용하는 것.

`Committed`
수정 사항들이 git에 저장이 된 상태를 "committed" 라고 하고 이러한 행위를 "commit" 한다고 표현한다.

### Git Basic Work Flow

>

1. 소스코드 전체를 다운로드 받는다 (전문적인 언어로는 "git repository를 checkout 한다").
2. 소스코드 파일들을 수정 합니다. 즉 개발을 한다.
3. 수정한 파일들을 stage 한다.
4. 그리고 계속 해서 소스코드 파일들을 수정해 나간다.
   해당 작업이 완료될때까지, 즉 commit 할 준비가 될때까지, 3,4번을 반복합니다.
5. 완료되면 commit 한다.

<br>
<br>

## Basic Git Commands

`git init`
프로젝트를 git repository로 만들기 위해서 사용하는 명령어. 여기서 프로젝트(project)라 함은 개발하고자 하는 소스코드들이 있는 디렉토리를 말한다. git init을 해서 git repo로 만들어야 git으로 버전 관리가 시작된다.

`git add`
수정 사항들, 즉 modified 파일들을 staged 상태로 옮기는 명령어. 그리고 git repo에 새로 추가된 파일들을 staged 상태로 옮길때도 사용된다. 새로 추가된 파일들은 "untracked" 파일 이라고 하는데, git에서는 이들도 수정 사항이라고 본다.
`git add .` 수정된 모든 파일을 staged 상태로 옮긴다는 의미

`git commit`
staged 된 파일들을 commit 할때 사용하는 명령어

`git diff`
개발 하고 있는 파일에 어떤 수정 사항들이 적용되었는지 보고싶을때 사용하는 명령어. 참고로 staged 된 수정 사항들은 git diff로 볼 수 없고, Modified 된 파일들만 git diff로 볼 수 있다.
(즉 git add 전에만 사용 가능 )

`git status`
현재 상태를 보여주는 명령어. 어떠한 파일들이 modified가 되었고 어떠한 파일들이 staged가 되었는지 등의 전체적인 상황을 보여준다.

`git log`
Commit 내역들을 보여주고 Commit history라고도 한다. git log를 통해 이제까지 커밋 내역들을 전부 볼 수 있다. 다만 출력되는 포맷이 보기가 쉽지가 않아서 tig 같은 tool을 사용하면 훨씬 편리하다.

`git rm`
원하는 파일을 git repo에서 삭제한다.

`git mv`
원하는 파일을 git repo 상에서 이동 시킬때 사용한다. 주로 rename 할때 사용 된다.

`git branch`
Branch를 생성할 때 사용된다. Branch에 관해서는 아래를 참고하기

`git checkout`
어떤 branch를 checkout 할때 사용되는 명령어

<br>

# 3. Branch & Merging

Branch는 나뭇가지 혹은 분점 을 뜻한다. 즉 기본이 되는 큰 줄기가 있고 그 줄기로 부터 옆으로 가지가 나는걸 의미하는데, Git의 branch 모델이 바로 이러한 구조이다. 먼저 git에서 기준이 되는 `master branch`가 있다. 그리고 각 개발자는 master branch를 checkout 먼저 하고, master branch로 부터 자신만의 branch를 만든다. 이걸 `feature branch`라고 한다. 그리고 feature branch를 기반으로 개발을 한 후 개발이 완료가 되고 commit을 하면 자신의 `feature branch`를 다시 `master branch`로 합하게 된다. 이렇게 합하는 과정을 `merge` 한다고 말한다.

### branch 활용하는법 정리

1.  `Master branch`를 `checkout` 한다.
2.  자신만의 `feature branch`를 만든다.
3.  Feature branch에서 개발을 한다.
4.  완료되면 `commit` 한다.
5.  Master branch에 feature branch를 `merge` 한다.

![](https://images.velog.io/images/rimu/post/e2a71e10-2a9d-4edf-aa8c-ff63fe3e584c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-04-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.38.47.png)

## Basic Git Branch Commands

`git branch 브랜치명`

`git checkout feature/login`
브랜치 이동하기 (checkout)

`git branch`
현재 브랜치 확인하기
현재 체크아웃된 브랜치와 내가 갖고 있는 브랜치 목록을 확인할 수 있음

`git branch -D feature/login`
브랜치 삭제하기

<br>

_이글은 위코드 부트캠프의 소중한 공부자료를 보고 정리한 내용입니다._
