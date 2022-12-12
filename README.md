# 참고) checkout 명령어가 Git 2.23 버전부터 switch, restore로 분리

## 과거로 돌아가기 => RESET과 REVERT의 차이점

- RESET은 돌아가고자 하는 그 해시코드로 돌아간다 (기록이 남지 않는다.)
- REVERT는 되돌릴 해시코드를 지정한다 (기록이 남는다.) => 협업시에는 이것을 사용할것!

## revert를 할때 충돌이 날때 대처방법

1. 충돌이 난 경우, 내가 어떠한 것을 할지 선택한다(파일을 지울지, 수정을할지 등등) 
2. 다 완료되면 
```js
    git revert --continue
```
라고 쳐준다. 추가로, revert과정을 취소하고 싶을떈
```js
    git reset --hard
```
이렇게 작성하면 가장최근의 커밋내용으로 리셋이된다.

## git log 명령어를 치면 브랜치 상관없이 모든 내역이 나오는가?

- 답은 NO다. git log를 치면 branch별로 각각의 히스토리가 나오게된다.
## 병합하는 두가지 방법 : merge vs rebase (1)

- merge : 브랜치의 이력이 남는다.
- rebase : 브랜치의 이력이 남지 않아 history가 깔끔해지지만, 협업하는 과정에서는 이 방법은 지양하는 것이 좋다.

## 병합하는 두가지 방법 : merge vs rebase (2)
- 상황 : main 브랜치에다가 development 브랜치 작업을 병합하고 싶을때
- merge : main 브랜치로 이동 => git merge development 
- rebase : development 브랜치로 이동 => git rebase main => main브랜치로 이동 후 => git merge development (fast-forward하기)

## merge를 할때 충돌이 났을때
- 너무 복잡하여 되돌리고 싶을땐
```js
    git merge --abort
```
이렇게 해준다.

- 충돌작업을 해결하고 싶다면
```js
    충돌 부분 직접 수정 후 => git add . => git commit 으로 merge 완료
```

## rebase를 할때 충돌이 났을때
- 너무 복잡하여 되돌리고 싶을땐
```js
    git rebase --abort
```
이렇게 해준다.

- 충돌작업을 해결하고 싶다면
```js
    충돌 부분 직접 수정 후 => git add . => git rebase --continue => 커밋 (해결할때까지 처음부터 반복)
    완료되면 main(master) 브랜치로 가서 fast-forward하기
```

## github 명령어 모음
```js
    git remote add origin (원격 저장소 주소)
    => 원격을 추가한다 그 이름은 origin이고 그 주소는 (원격 저장소 주소) 이다.
```
```js
    git branch -M main 
    => 로컬, 원격의 기본 브랜치 명을 main으로 한다.
```
```js
    git push -u origin main
    => 로컬 저장소의 커밋 내역들을 원격으로 push한다(업로드한다.)
    => 이 다음부터는 git push를 해도 origin의 main 브랜치에다가 push를 한다.
```
```js
    git remote 또는 git remote -v
    => 연결된 원격의 상세정보 알아보기.
```
```js
    git remote remove (origin 등 원격의 이름)
    => 원격 지우기 (로컬 프로젝트와의 연결만 없애는 것. GitHub의 레포지토리는 지워지지 않음)

    c.f) 원격의 브랜치를 삭제하고 싶을 때
    git push (원격 이름) --delete (원격의 브랜치명)
    ex) git push origin --delete jinwoong-branch
```
## 내가 커밋한 것도 있고, 상대방이 커밋한 것도 있을때 (pull => push 순서)
```js
    1. pull(merge 방식) => git pull --no-rebase ===> 기록이 남는다.

    2. pull(rebase 방식) => git pull --rebase ===> 원격의 커밋을 먼저 붙이고 그다음 내 커밋을 뒤에다가 붙인다. 기록이 남지 않는다.
```
## 로컬의 코드를 원격에다가 강제적용할때
```js
    git push --force (주의할것!)
```
## 로컬에서 내가 만든 브랜치를 원격에다가 추가하고 싶을 때
```js
    로컬의 브랜치 이름이 jinwoon-branch라면,,,
    => git push -u origin jinwoon-branch
```
## 원격의 브랜치를 내 로컬로 받아오고 싶을 때
```js
    원격의 브랜치 이름이 jungsik-branch라면,,,
    1) git fetch 하고 git branch -a (또는 --all로 브랜치가 추가되었는지 확인)
    2) git switch -t origin/jungsik-branch
```
## add한 파일을 working directory로 내리고 싶을때
```js
    git restore --staged 파일명
    c.f) 그렇다면 add가 된 파일을 아예 취소하고 싶을때는 (git restore 파일명)을 하면 되지 않을까? => 답은 NO.
    (git restore 파일명) 으로 수정내역을 취소하고 싶으면 해당 파일이 스테이지에 올라가 있으면 안된다. 하지만 번거롭게 이렇게 하기보다는 GUI형식을 이용하자.
```
## checkout 명령어는 언제쓰는가?
```js
    코드 변경없이 단순히 시간여행을 할 수 있다.
```

 ## git checkout 으로 시간여행을 하다가보면, branch명이 해시코드로 되어있는 것을 확인할 수 있다. 이로 보았을때 checkout으로 이동하는 HEAD는 일종의 새로운 브랜치라고 할 수 있다. 
![non-zero vs even odd](./images/1.png)

## 그래서 원래 기존의 최근 시점으로 이동할 때는 브랜치 이동하듯이 하면된다.
```js
    git switch 이동하고자 하는 브랜치name
```
## 원격에 있는 작업사항을 일단은 보고만 싶을때 (fetch)
```js
    1) git fetch [ c.f) fetch는 모든 브랜치의 변경사항들을 일단 받아만온다. ]
    2) git checkout origin/main (origin/master)으로 변경사항들을 확인한 다음
    3) git switch master로 로컬의 브랜치로 이동한다.
    4) 변경사항을 pull한다.
```
## git config
```js
    git config --list  ===> git의 config list들 출력
    git config -e  ===> 에디터로 git config를 보고싶다면~
    (c.f vscode로 열고싶다면 => git config --global core.editor "code --wait")
    
    1) git pull의 기본전략 (merge로 할지, rebase로 할지)
    git config pull.rebase false => merge
    git config pull.rebase true => rebase

    2) 기본 브랜치명
    git config --global init.defaultBranch main

    3) 로컬 브랜치와 원격 브랜치명을 동일하게 설정하길 원한다면
    git config --global push.default current

    4) git 명령어 단축키로 설정하기 (ex commit -am 을 'cam'으로)
    git config --global alias.cam 'commit -am'

```

## git 기록 컨벤션

```js
(작성법)
type: subject

body (optional)
...
...
...

footer (optional)

=================================================

(예시)
feat: 압축파일 미리보기 기능 추가

사용자의 편의를 위해 압축을 풀기 전에
다음과 같이 압축파일 미리보기를 할 수 있도록 함
 - 마우스 오른쪽 클릭
 - 윈도우 탐색기 또는 맥 파인더의 미리보기 창

Closes #125

```
![non-zero vs even odd](./images/2.png)


## git add, git commit 좀더 세심하게 다루기
```js
    1) git add -p (git add를 하나하나 내 변경사항을 확인하며 add를 하고 싶을 때)
    2) git commit -v (git commit할때 수정사항을 확인할 수 있다.)
    3) git diff --staged (스테이징으로 올라간 변경사항과 비교하기)
```

## 작업중이던 코드 한곳으로 치워두기(git stash)
git stash로 치워둔 작업코드는 어느 브랜치에서든, 어떤 커밋해시에서든 코드를 적용시킬 수 있다.

```js
    git stash list ===> stash list들 보기
    git stash -p ===> git이 나에게 일일이 물어보면서 stash할 부분을 체크한다.
    git stash -m "메모" (또는) git stash save "메모" ===> stash에 메모를 남기고 싶을때
    git stash apply "stash id" ===> 특정 stash작업물들을 다시 적용시키기
    git stash drop "stash id" ===> 특정 stash작업물들을 삭제하기
    git stash clear ===> git stash list들 모두 지우기
    git stash pop "stash id" ===> 특정 stash작업물들을 적용하고 해당 stash 내역은 삭제한다.
```

## 기존의 커밋을 수정하기
1) 바로 직전의 커밋을 수정하고 싶다면 ===> git commit --amend








