---
title:  "GIT"
excerpt: "DevOps 부트캠프 Section 1 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-03-20
last_modified_at: 2023-03-20
---
# GIT
2005년, 리누스 토르발스(Linus Torvalds)는 깃(Git)을 처음 세상에 소개하면서 '깃은 지옥에서 온 관리자'라 말하였다. 그가 말한 '지옥은' 어디였을까? 26년 동안 1만 명이 넘는 소프트웨어 엔지니어들이 오픈 소스 방식으로 2천만 줄이 넘는 컴퓨터 소스코드를 작성했다면 바로 그것이 지옥이 아니었을까? 이러한 지옥에서 태어난 소프트웨어가 바로 리눅스(linux) 운영체제이다. 


지옥같은 작업환경에서 벗어나기 위해 만든 시스템이 바로 깃이다. 리눅스를 만드는 개발자들은 깃을 통해 리눅스의 수 많은 소스코드를 효율적으로 관리하기 시작하였다. 오늘날에는 리눅스 뿐만 아니라 수많은 소프트웨어의 소스코드가 깃을 통하여 관리되고 있다.

즉, 깃은 소스코드 관리를 위한 분산버전 관리 시스템이다.

## 깃(Git)의 장점
- 빠른속도, 단순한 구조, 비 선형적인 개발, 완벽한 분산, 대형 프로젝트에도 유용<br><br>

## 깃(Git)이 제공하는 핵심기능 
- 버전관리(Version Control) <br>
   문서를 수정할때마다 언제 수정했는지, 어떤것을 변경했는지 편하고 구체적으로 기록하기 위한 버전 관리 시스템.

- 백업(Backup) <br>
   컴퓨터는 언젠간 고장나기 때문에 자료가 유실되는 것을 방지 하기위해 백업( 현재 컴퓨터에 있는 자료를 다른 컴퓨터에 복제 )을 해야한다.<br>
   백업 공간을 제공하는 인터넷 서비스 중에서 깃 파일을 위한 것도 여럿있다. <br>
   이를 깃의 원격 저장소 또는 온라인 저장소라 한다. 이러한 서비스중 가장 많이 쓰이는 것이 깃허브(GitHub) 이다.
- 협업(Collaboration) <br>
   깃을 사용하면 팀원들이 파일을 편하게 주고받으면서 일할 수 있습니다. 
   <br>또는 누가 어느 부분에서 어떻게 수정했는지 기록에 남기 때문에 나중에 오류가 생겼을 때도 파악하기 쉽습니다.


## 깃허브(GitHub)
깃허브는 소프트웨어 개발 프로젝트를 위한 소스코드 관리서비스(원격 저장소) 이다.<br>
소스코드 열람 및 간단한 버그정리, 버전관리 그리고 sns기능도 있는 호스팅 플랫폼 서비스이다. <br>
그래서 함께 개발한 소스코드를 공유할때 또는 상대로부터 받은 코드를 수정할때 굉장히 유용하게 쓰인다.


깃허브는 깃을 웹에서 보다 편하게 쓸 수 있도록 만든 도구이다.<br>
즉, 깃을 활용해서 짠 코드를 공유할 수 있는 공간이라 생각할 수 있다. <br>
예를 들어 깃에서는 명령어를 하나하나 입력해야 하지만, 깃허브는 웹그래픽 기반의 도구이기 때문어 서비스를 더 직관적으로 이해할 수 있다.

💬원격 저장소에서 깃을 사용할 수 있다.
-  지역 저장소를 만들지 않아도 깃허브에 원격 저장소를 만들어 사용 할 수도 있고, 지역 저장소가 있다면 원격 저장소와 연결해서 사용 할 수도 있다.

💬 지역 저장소를 백업할 수 있다.
- 깃허브에 원격 저장소를 만들고 사용자 컴퓨터의 지역 조장소를 연결한 후 동기화를 하면 지역 저장소를 인터넷상에 백업할 수 있다. <br>
깃허브에 백업하면 원격 저장소에 손쉽게 커밋할 수 있다.

💬 협업 프로젝트에서 사용할 수 있다.
- 팀 프로젝트를 진행할 때도 이젠 깃허브가 기본 저장소가 된다. <br>
인터넷만 가능하면 누구나 쉽게 접근할 수 있고, 깃과 깃허브에서 여러 가지 협업 도구를 제공하기 때문에 깃허브를 사용하면 여러 명의 팀원이 하나의 프로젝트를 진행하기 쉽다.

💬 자신의 개발 이력을 남길 수 있다.
- 깃허브에서 소스를 수정하고 오픈 소스에 참여해서 하는 일들은 사용자 초기 화면에 날짜별로 모두 기록에 남는다.<br> 지원자가 어떤 주제에 관심이 많은지, 어떤 것들을 개발했는지, 그리고 무엇을 개발 중인지 한눈에 확인할 수 있기 때문에 깃허브는 개발자가 자신의 개발 이력을 관리하기 좋은 플랫폼이다.

💬 다른 사람의 소스를 살펴볼 수 있고, 오픈 소스에 참여할 수도 있다.
- 개발자로서 실력을 높이는 방법 중 하나가 다른 사람의 소스를 읽어보고 분석하면서 나름대로 소스를 수정하고 작성해 보는 것이다.<br>
깃허브는 전세계 개발자들이 공개해 놓은 소스들이 많다. 이런 오픈소스를 살펴보고 참여할 수 있는것도 깃허브에 커다란 매력이다.<br><br>

## GitHub 사전지식
1. 로컬 저장소(local)와 원격 저장소(remote)

git 저장소는 자신의 컴퓨터인 로컬 저장소와 서버에 있는 원격 저장소로 나뉜다. local에서 작업한 것은 remote로 push해줘야만 변경사항이 서버에 반영된다. 

2. add, commit, push

자신이 작업한 내용을 remote 저장소에 반영하기 위해서는, 변경사항을 추가하고(add), local에 저장하고(commit), remote에 업로드(push) 해야한다.

3. branch

여러 개발자들이 공동으로 작업할 수 있게 기본 master branch에서 새로운 가지를 만들어 독립된 공간에서 작업을 수행할 수 있다. 이 때 주기적으로 변경사항을 합치는 것이 필요하다.  

4. pull

remote에 있는 내용을 local에 받는 과정이다. 이때 현재 자신의 branch가 어디인지 확인을 잘 하고 pull하도록 한다. 만약 자신의 local에 변경사항이 있다면 pull할 시 에러가 나므로 add, commit을 진행한 후 pull하거나 stash하여 자신의 변경사항을 다른곳에 저장한 후 pull하도록 한다. 

5. 기본 흐름

github 공간 만들기(clone, init) => 파일 작성 => 파일의 변경사항 임시저장(add) => local에 저장(commit) => remote에 업데이트(push) => 로컬 업데이트(pull) => add => commit => push ==> pull...(반복)

6. 로컬 작업 시작 전 무조건 pull

remote 저장소에 변경된 사항이 있을 수 있기에(여러명이서 작업시) 무조건 파일을 건들기 전에 pull하도록 한다. 안그러면 conflict이 일어나 수동으로 고쳐야한다. <br><br>

## Git 명령어
### 자주 사용하는 명령어
1. git clone https://~

깃허브에서 project를 만든 후 git clone하여 local에도 작업공간을 만든다. 
   - 1-1. git init
   <br>깃허브에서 project를 만들어 clone하지 않고 컴퓨터에서 작업을 먼저 시작했을 때 저장소를 생성한다. 

 

2. git branch

현재 branch(*표시 되어있는) 및 local branch 확인

- 2-1 git branch wngud <br>
wngud branch 만들기

- 2-2 git branch -d wngud<br>
wngud branch 삭제

- 2-3 git branch -D wngud <br>
wngud branch 강제 삭제(merge가 되지 않은 branch에 대한 삭제)

- 2-4 git push origin -d wngud <br>
remote 서버에 있는 wngud branch 지우기

- 2-5 git push origin :wngud
로컬에서 wngud branch 지웠을 때 remote에도 그 변경사항 반영하기

 

3. git checkout wngud<br>
wngud branch로 이동

- 3-1 git checkout -b wngud <br>
wngud branch 만들고 이동

 

4. git status <br>
현재 상태(add전후, commit 전후 등 확인 가능) 및 브랜치 확인

 
5. git add, git commit, git push, git pull

- git add .: 변경사항 모두( . )임시저장
- git push: remote 저장소에 변경사항 반영
- git commit -m "메세지 내용" :
commit 하여 local 저장소에 반영

6. git log<br>
로컬 저장소의 commit history 보기

- 6-1. git log -n 10 <br>
10개만 보기

- 6-2. git log --oneline --graph<br>
log 그래프로 확인

7. git checkout -- wngud.py <br>
변경된 wngud.py 되돌리기

8. git rm wngud.py <br>
wngud.py 로컬, git 저장소 모두에서 삭제

- 8-1. git rm --cached wngud.py<br>
wngud.py를 git 저장소에서 삭제 (로컬은 유지) 

[출처](https://jayeon8282.tistory.com/4): https://jayeon8282.tistory.com/4

## 브랜치(Branch)
모든 버전 관리 시스템에는 브랜치(Branch)라는 개념이 있다. 브랜치는 원래 나뭇가지라는 뜻이다. 버전 관리 시스템에서는 나무가 가지에서 새 줄기를 뻗듯이 여러 갈래로 퍼지는 데이터 흐름을 가르키는 말로 사용한다.<br><br>

브랜치는 독립적으로 어떤 작업을 진행하기 위한 것이다. 필요에 의해 만들어지는 각각의 브랜치는 다른 브랜치의 영향을 받지 않기때문에, 여러작업을 동시에 진행할 수 있다. 즉, 자유롭게 일할 수 있고 협업을 쉽고 편리하게 할 수 있도록 도와준다.


### 분기
새 브랜치를 만드는 것을 분기(Branch)라 한다.
![alt text](/images/branch.png)


### 병합
분기(새 브랜치를 생성)했던 브랜치를 marster 브랜치에 합치는 것을 병합(merge)이라 한다
![alt text](/images/merge.png)

<br><br>

## Git 관계도
![alt text](/images/gitdata.png)

- workspace(work tree) <br>
현재 작업중인 장소 ( 내컴퓨터상)

- index(stage)<br>
workspace의 수정된 소스를 저장하는 장소(add) - commit 전

- local repository<br>
원격 저장소 업로드하기 전(commit까지만) 저장소(push) <br>
workspace 반영 없이 원격 저장소의 수정사항을 적용할 수 있는 장소 (fetch)
- remote repository<br>
원격 저장소<br>
clone 명령어를 통해 github(remote repos) 를 work space로 가져올 수 있음.<br><br>

=> index 와 stage의 관계는?
- local 과 workspace 사이 공간인 '인덱스'에 파일 상태를 기록하는 것을 staging 이라고 한다.

=> pull 과 clone 은 모두 github에서 workspace로 파일을 가져오는 것이다. 그럼 그 둘의 차이는 무엇인가?

- git pull : 원격 저장소의 내용을 가져와서 현재 브랜치와 병합하는 것 ( 내가 workspace에서 작업하던 것은 유지)

- git clone: 원격 저장소의 내용을 가져와서 현재 브랜치에 덮어쓰기 ( 내가 workspace에서 작업하던 것 유지 X) 

