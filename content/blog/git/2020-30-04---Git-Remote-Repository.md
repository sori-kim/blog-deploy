---
title: '[Git] Git Remote 저장소와 Local 저장소 연결하여 관리하기'
date: 2020-04-30 04:12:00
draft: false
category: 'git'
---

# Remote 저장소와 Local 저장소

앞서 살펴봤던 Git의 저장소(repository)는 두가지 종류가 있는데, `Remote repository`와 `Local repository`가 바로 그것이다.  
Git은 개인적으로 코드의 변경사항을 추적하고 프로젝트를 관리하기 위해 사용할 뿐만 아니라 다른 사람들과 협업하여 개발할때 역시 아주 중요한 역할을 담당하고 그 부분을 위해 등장하는게 바로 remote repository(원격저장소)이다.

이 원격저장소를 위해 주로 사용되는 것은 `Github`인데, git을 효율적으로 관리하기 위한 원격서버이자 클라우드 역할을 담당한다. 평소에는 자신의 PC의 Local repository에서 작업하다가 자신의 소스코드를 공개하고 싶을 때, 혹은 다른사람들과 협업을 시작해야할때 Remote repository에 개발한 소스코드를 업로드한다. (`Push`) 내 소스코드를 올리는 것 뿐만 아니라, Remote repository에서 다른 사람의 소스코드를 자신의 Local repository로 가져올 수도 있다. (`Pull`)

![](https://images.velog.io/images/rimu/post/7368df43-dd64-4d1e-abf8-b5a3d228c970/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-04-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.27.33.png)
<br>

![](https://images.velog.io/images/rimu/post/cc53a881-aac4-438b-9a6f-ad49b8b1bfa8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-04-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.30.40.png)

## Remote 저장소와 Local 저장소 연결하기

1. github에서 새로운 레파지토리를 생성한다.

2. `$ git remote add origin 레파지토리주소`
   가령 내가 만든 westagram이라는 레파지토리를 로컬과 연결하고 싶다면 <br>
   \$ git remote add origin https://github.com/rimuuu/westagram.git 이라고 입력한다.
   참고로 여기서 origin은 큰 의미 없는 git을 구분하기 위해서 사용하는 이름표 같은 것이다.
   내가 원하는 아무 단어로 바꿔도 아무 상관없다.

3. remote 저장소가 잘 연결되었는지 확인해보고싶다면
   `$ git remote -v`라고 입력해보면 현재 내 로컬저장소와 연결된 remote저장소의 목록을 볼 수 있다.
   내가 개발하고 있는 westagram 폴더에 가서 명령을 입력해보니 내 remote 저장소의 목록이 나왔다.
   ![](https://images.velog.io/images/rimu/post/ffb89da6-9eb5-4b12-beef-05b2a6a20e62/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-04-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.42.11.png)

4. 개발을 하다가 이 시점을 기록하고싶다면 `git add` `git commit`의 과정을 거친다.

5. `git push origin master`라고 입력하면 내 remote저장소로 push가 된다.

## 브랜치를 넘나들며 작업하고 Remote 저장소에 올리기

### Branch

모든 작업 내용을 master에만 푸쉬하면 너무 잦은 충돌과 소스가 뒤엉키는 문제가 생긴다.
그래서 최대한 기능별로 branch를 나눠서 작업을 하는게 필요하다.

흔히 branch를 딴다고 말하는데, 브랜치를 딴다는건 이름표만 바꿔서 특정시간대로 프로젝트를 복제하는거다.
브랜치를 따서 작업해서 좋은점은 특정시점의 복제본을 갖고 있기 때문에 기준점이 있어서 검토 작업을 할때 혼란이 덜하다. 그 외에도 아래와 같은 장점을 갖고 있다.

> 1.다른 사람들과 협업할때 같은 부분을 다르게 개발하여 발생하는 충돌을 줄일 수 있다. 2.기능별로 정해진 부분만 작업할 수 있어서 프로젝트 관리가 수월하다.

브랜치를 넘나들며 작업을 하고 싶을때는 아래와 같은 순서를 따르면 된다.
일반적인 git workflow를 벗어나지 않되, branch를 생성하고 checkout하고 push할때 branch 이름을 적용하는 정도만 달라진다.
`git branch feature/comment`
네이밍을 할때는 기능별로 나누기때문에 feature라고 시작하는게 일반적인 방

`git branch` 브랜치가 잘 생성되었는지 본다

`git checkout feature/comment` 기준 브랜치를 바꾼다. (작업영역을 바꾼다)

`git add .` 작업의 변경사항을 스테이지에 올린다.

`git commit -m “”` 커밋한다.

`git push origin feature/comment` 브랜치의 변경사항을 원격에 올린다.

이제 원격에 새로운 브랜치가 생성되고 변경사항이 반영이 된다.

---

깃 짱짱맨! 이해하고 나니 깃 넘 재밌다.ㅎㅎㅎ (아직까지는🙄?) <br>
이제 1일 1커밋 가보즈아~<br>
![](https://images.velog.io/images/rimu/post/83839fa9-f5d0-43a5-8390-3c24d2065ded/IMG_1840.GIF)

<br>

_이글은 위코드 부트캠프의 git 세션을 듣고 정리한 내용입니다._
