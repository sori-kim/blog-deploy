---
title: '[Git] Github에서 fork한 저장소 최신으로 유지하기'
date: 2020-06-29 01:00:03
draft: false
category: 'git'
---

## 오픈소스 기여의 시작은 github fork 부터

Github에 있는 오픈소스 프로젝트에 참여하는 경험이 개발자에게 있어서 중요하다고 이야기를 들은 적 있지만 막연하게 먼 미래의 일이 될거라고 생각했는데.... 운 좋게도 이번에 인턴으로 생활하게 된 dooboolab에서 주요하게 펼치고 있는 활동이 react native 커뮤니티 활성화를 위한 오픈소스 프로젝트였다. 게다가 그 프로젝트에 우리가 참여하게 되면서 이번 기업협업 프로젝트의 가장 중요한 과제로 **오픈소스 기여와 친해지기** 로 정했다.

## fork

fork는 어떤 원본 저장소를 내 github 저장소로 복사하는 것인데, 내 저장소에 복사된 파일들은 내 마음대로 수정해서 업데이트 할 수 있다.
만약 내가 만든 업데이트가 원본 프로젝트에 도움이 된다고 생각하면 Pull request를 보내서 원본 저장소를 작업하고 있는 개발자들에게 내 코드를 가져가라고 요청하는 것이다.

문제는 fork를 한 특정 시점의 저장소 내역을 복사한 뒤, 원본 저장소는 계속해서 업데이트가 되고 있을텐데 이걸 내가 fork한 복제본에도 어떻게 반영할 수 있는가에 대한 부분이었다. 이 부분을 앞으로 자주 사용하게 될 것 같아서 기록해두고 필요할때마다 보려고 한다.

## upstream으로 원본 repo를 remote에 연결하기

원본 레포 fork -> fork된 레포 git clone -> 내 PC에 파일들을 내려받아서 작업을 하다가, 원본 레포의 최신화를 하고 싶을때의 상황이라고 가정해보자.

이미 git clone을 했기 때문에 remote repo의 origin은 fork하여 복제한 레포로 저장되어 있을 것이다.
여기에 업데이트가 진행중인 원본 레포를 upstream이라는 이름으로 새로운 remote repo로 관리하는 것이다.

```
git remote add upstream https://github.com/dooboolab/dooboo-ui.git
```

실행 후 git remote -v 를 입력해보면 내 remote repo들을 확인해볼 수 있고 upstream repo가 잘 추가가 되었음을 알 수 있다.
![스크린샷 2020-06-29 오후 2 32 02](https://user-images.githubusercontent.com/60246689/85976377-67656800-ba15-11ea-882f-14e8717aff51.png)

참고로 upstream 외에 다른 이름을 이용해도 무방한데 컨벤션인 것 같으니까 되도록 따라보자.

## upstream fetch

이제 remote가 연결되었으니 업데이트가 진행중인 원본 레포의 최신본을 받아올 수 있다.

```
git fetch upstream
```

이 과정을 통해 원본 레포 master branch에 있는 내역들이 내 PC의 upstream/master로 복사가 된다. <br>
그 다음에는 upstream/master에 있는 작업내역들을 내 master로 merge하여 합친다.

```
git checkout master
git merge upstream/master
```

마지막으로 로컬의 변경사항을 원격에도 반영해야 하기 때문에, git push origin master를 하여 내 forked repo와 원본 repo가 동기화 되도록 하면 끝!

<br>
<br>

만약 merge하기전에 에러가 뜬다면 다음의 포스팅을 참고하여 따라가보기. <br>
[블로그 링크]('https://lifove.tistory.com/54')
