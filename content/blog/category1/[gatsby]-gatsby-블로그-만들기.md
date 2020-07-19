---
title: '[Git] Git을 편하게 쓸 수 있는 툴 tig'
date: 2020-05-01 13:07:16
category: 'category1'
draft: false
---

## 1. Tig: text-mode interface for Git

리눅스에서 git을 쓰다 보면 GUI가 없는 구조이기 때문에 답답할 때가 있다. 가령 특정 파일의 변경 이력을 추적할 때 터미널에서 보여지는 일반적인 화면으로는 그 내용을 파악하기가 쉽지않다.
이를 극복할 수 있게 도와주는 툴이 바로 Tig이다.

tig는 텍스트 기반의 Git 유저 인터페이스인데, 이 글에서 tig의 설치와 간단한 사용법에 대해 정리해보려고 한다.

## 2. tig 설치

[접속하기](https://jonas.github.io/tig/)

위의 사이트에 접속하면 tig 를 설치하는 메뉴얼 페이지가 나온다. tig의 장점 중 하나가 가볍기 때문에 간단히 설치할 수 있다. 나는 더 간단하게 brew를 이용해서 설치했다.

```sh
$ brew install tig
```

## 3. tig 사용하기

```sh
$ tig
```

git log 대신 tig 명령어를 사용하면 commit한 내역을 좀 더 쉽게 볼 수 있다.
방향키를 사용해서 내가 자세히 보고싶은 commit 내역에 들어가서 볼 수 있다.

<br>

![스크린샷 2020-05-01 오후 1 05 24](https://user-images.githubusercontent.com/60246689/80792708-1fba8f80-8bd0-11ea-9997-271ad66cbfb1.png)

<br>

```sh
$ tig status
```

tig status를 입력한 상태에서 h 버튼을 누르면 단축키 정보를 확인할 수 있다. 이 정보를 토대로 필요한 기능을 익혀 사용하면 된다.

![스크린샷 2020-05-01 오후 5 28 53](https://user-images.githubusercontent.com/60246689/80793144-462cfa80-8bd1-11ea-8e0b-3d944a1541b5.png)
