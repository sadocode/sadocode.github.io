---
layout: post
title:  "Git 기초 사용법"
date:   2019-06-09
excerpt: "How to use Git bash."
tag:
- Git
comments: true
---


> 최근에 입사한 회사에서 git으로 형상관리를 하는 것을 보고 공부를 시작했다. 그런데 간만에 git을 다루려니까 은근 헤매게 되길래 기본적인 것들을 정리해보려고 한다.

<br>

### 초기설정

git을 사용할 때 처음에 설정해줘야하는 부분이다.  
~~~
< > : 실제로 명렁어에는 들어가지 않는 괄호


git init <directory>
	: 해당 directory를 git repository로 이용한다.

git config --global user.name <"name">
	: user.name을 설정한다. 
git config --global user.email <email>
	: user.e-mail을 설정한다. 
	
	: --global 옵션을 쓰지 않으면, git repository마다 다른 name과 e-mail을 설정할 수 있다.

git config -- list
	: config 설정값들을 확인할 수 있다.

git config --unset user.name
git config --unset user.email
	: --unset 옵션을 사용하면 설정값을 지울 수 있다.

git remote add <repository name> <url>
	: url은 가져올 원격 저장소의 주소이고, repository name은 원격 저장소의 별칭이라 생각하면 된다.

git clone <url> <directory>
	: url(원격저장소)의 이력을 해당 로컬 directory에 그대로 복제한다.
~~~

<br>

### log

log를 통해서 로컬의 변경이력을 확인할 수 있다. 보통 merge나 commit 등의 작업 전후에 확인을 많이 한다. 예전 이력으로 돌아가고 싶을 때도 확인을 한다.
~~~
git log
	: 로컬의 변경 이력을 확인한다.
git log --graph --oneline
	: 로컬의 변경 이력을 그래프형태로 commit message만 따와서 확인한다.
~~~

<br>

### add, commit

로컬 저장소는 **working directory**, **index**, **HEAD** 라는 3영역으로 나누어져 있다. working directory에서 add를 하면 index로 넘어가게 되고, index에서 commit을 하면 HEAD로 넘어가서 최종 확정이 된다.
<br>
* working directory : 실제 파일들로 이루어진 작업 영역
* index : 준비 영역(staging area)
* HEAD : 최종 확정본(commit)

~~~
git add <file>
	: file을 add한다.
git add .
	: 변경, 수정, 삭제한 file을 모두 add한다.

git commit -m <"message">
	: add에 대한 설명을 담은 메시지를 commit한다.

git status
	: 현재 git 관리 하에 있는 폴더의 작업트리와 인덱스 상태를 확인한다.
	: (add 전 상태인) 변경, 수정, 삭제된 file과 (add된 상태인) index에 위치한 file을 확인할 수 있다.

git reset <file>
	: file의 add취소
git reset
	: index에 대기 중인 모든 file의 add 전체 취소
~~~

<br>

### branch

처음에 git init을 하게되면, master branch가 기본적으로 생성이 된다. master branch 외에도 사용자가 지정해 여러 branch들을 생성, 삭제하는 것들과 branch들을 병합하는 것이 가능하다.

~~~
git branch
	: 로컬의 branch 확인
git branch -r
	: 원격 저장소의 branch 확인
git branch -a
	: 로컬, 원격 저장소의 branch 모두 확인

git branch <branch name>
	: 로컬에 branch 생성
git branch -m <branch name> <new name>
	: 로컬의 branch명을 변경

git push --delete <repository name> <branch name>
	: 원격 저장소의 branch를 삭제 
~~~

<br>

### push, pull

add와 commit을 한다고 로컬 저장소의 내용이 원격 저장소로 넘어가는 것은 아니다. 로컬에서 변경된 이력을 원격 저장소에 반영하려면, push를 해줘야 한다.
~~~
git push <repository name> <branch>
	: 로컬의 branch를 원격 저장소에 push한다.
	: 원격 저장소에 이력을 반영하려는 branch가 로컬에만 존재하고 원격 저장소에는 없을 경우에는 아래와 같이 명령어를 입력해줘야한다.

git push <repository name>/<repository branch> <branch>
	: git push 원격 저장소명/원격 저장소 branch    로컬 branch

git pull <repository> <branch>
	: 원격 저장소의 해당 branch를 로컬로 가져온다.
~~~

<br>

### tag

commit을 할 때 message를 입력해줌으로써 각각의 이력을 구분할 수 있다. 하지만 tag를 입력해준다면, 더 간단하게 이력을 구분할 수 있어 유용하다.

~~~
git tag
	: 입력한 tag 목록 확인
git tag <tagname>
	: tag 달기
git tag -d <tagname>
	: tag 삭제

git push <repository> --tags
	: 원격 저장소에 로컬의 모든 tag를 반영한다.
~~~

<br>

### merge

merge는 branch를 합칠 때 사용한다. 예를 들어 master branch에는 배포 버전만 두고, develop branch에서 개발을 진행한다고 하자. 이 때, develop branch에서 개발이 완료되면 master branch에서 이를 병합한다.
merge에는 여러 방법이 있는데, 이번에는 fast-forward(빨리 감기)병합만 적었다.

~~~
git checkout master
	: master branch로 이동
git merge develop
	: master branch에 develop branch를 병합.
~~~

<br>

### 예전 버전 확인

git에 code를 올리다보면, 예전 commit의 code를 확인하고 싶을 때가 있다. 이 때 다음과 같은 방법으로 예전 버전으로 돌아갈 수 있다.

~~~
git checkout HEAD~n
	: n단계 전의 commit으로 돌아간다.

git checkout <branch>
	: 다시 특정 branch의 최근 commit으로 복귀한다.
~~~

<br>
