---
title: "[Git] Git merge와 Conflict 해결하기"
date: "2020-04-30T08:12:03.284Z"
template: "post"
draft: false
slug: "git/200430/3"
category: "git"
tags:
  - "git"
  # - "Typography"
  # - "Web Development"
description: "master브랜치에서 분화하여 기능별로 작업한 branch들이 최종적으로 개발이 완료되면 master브랜치에 합체를 해야한다.
이때 사용하는 개념이 `merge(병합)`이고, 다음의 순서를 거친다."
socialImage: "/media/42-line-bible.jpg"
---

# 1. Git Merge

master브랜치에서 분화하여 기능별로 작업한 branch들이 최종적으로 개발이 완료되면 master브랜치에 합체를 해야한다.
이때 사용하는 개념이 `merge(병합)`이고, 다음의 순서를 거친다.

### master 브랜치의 권한이 나에게 있는 경우

1. 마스터 브런치로 돌아오기
   `$ git checkout master`

2. git merge 뒤에 변화를 가져올 브런치 이름을 적는다.
   `git merge feature/login`

3. 커밋을 저장하는 vi editor화면이 뜨면 커밋내용 입력하고 :wq로 저장하고 나간다.
   <br>
   (i를 입력하면 입력모드가 시작되고 esc를 누르면 입력모드가 끝난다. 그 상태에서 :wq)

<strong>[참고]</strong>  
 `git log --graph --all --decorate`  
 두 브랜치에서의 작업내역을 시각화된 형태로 볼 수 있다.

### master 브랜치의 권한이 나에게 없는 경우

브랜치의 작업내역을 리모트 저장소로 푸쉬한 뒤 해당 깃헙 주소로 가보면 변경사항이 반영되었고, pull request에
`New pull request`를 할 수 있게 되어있다.

![스크린샷 2020-04-30 오후 9 51 27](https://user-images.githubusercontent.com/60246689/80712448-f98fe380-8b2c-11ea-9c04-a7630f7cbad7.png)

오른쪽 아래에 보이는 `Create pull request` 버튼을 누르면 PR을 할 수 있다
(PR은 한 브랜치 당 한 번 할 수 있음)

주의사항: 두개의 브런치에서 같은 파일에 대해 서로 다른 결과를 입력했다면 merge를 할때 conflict라는 에러가 발생한다. 두개의 내용 중 어떤걸 택할지 결정하고 다시 시도하라는 뜻이다.
그래서 특히 협업을 하거나 여러브런치를 나눠서 사용할때는 같은 파일에 대한 코드를 서로 다르게 입력하지 않도록 조심하는게 좋다.
둘중 하나를 택일해서 다른쪽에 대한 내용을 지워준 후 다시 커밋하면 해결된다. 이 부분은 아래에서 좀 더 구체적으로 보겠다.

## 2. Conflict 해결하기

실제로 협업하여 개발을 진행할때 merge가 그리 평화롭게 이루어지지 않는다.
앞으로 많은 conflict를 마주하게 될 것이고, 그 때마다 변경사항을 잘 골라서 다시 커밋 후 merge 해야한다.
그래서 conflict 해결하는 방법에 대해 꼭 기억해두려고 포스팅을 남겨둔다.

1. `$ git pull origin master` master 브랜치의 최신본을 원격에서 당겨오기

🚨 컨플릭트가 났다는 경고가 뜸 🚨

![IMG_2024](https://user-images.githubusercontent.com/60246689/80711817-f1837400-8b2b-11ea-8901-d95c51f58018.jpg)

2. `$ cd 파일위치` 컨플릭트가 난 파일이 있는 곳으로 간다.

3. `$ vi 파일명` 해당 들어가서 컨플릭트가 난 부분 중에 원하는 내용을 선택하여 수정, 저장

4. 이제 리모트 저장소에 업데이트 내용을 업데이트 해줘야한다.

`git add`

`git commit`

`git push origin feature/login`

이후 다시 merge하면 해결!
근데 그 사이에 다른 사람들이 먼저 수정을 했다면 또 컨플릭트가 날 수도....

<!-- - [The first transition](#the-first-transition)
- [The digital age](#the-digital-age)
- [Loss of humanity through transitions](#loss-of-humanity-through-transitions)
- [Chasing perfection](#chasing-perfection) -->

<!-- _Originally published by [Matej Latin](http://matejlatin.co.uk/) on [Medium](https://medium.com/design-notes/humane-typography-in-the-digital-age-9bd5c16199bd?ref=webdesignernews.com#.lygo82z0x)._ -->
