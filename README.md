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
1) 바로 직전의 커밋을 수정하고 싶다면 ===> 같이 commit할 작업들을 수행해주고(git add 등) => "git commit --amend"

2) 아싸리 통으로 커밋히스토리를 만지고 싶다면,,, ===> git rebase -i '돌아갈 커밋해시'
![](./images/3.png)

```js
    e, edit => 커밋 쪼개기
    d, drop => 커밋 자체를 삭제하기
    s, squash => 커밋합치기


    커밋 쪼개는 방법은,,,
    1) git rebase -i "해결하고 싶은 커밋 직전의 커밋해시"
    2) 해당 커밋에 e, edit 선택
    3) git reset HEAD~로 커밋직전으로 또 이동하기
    4) 변화들을 따로 커밋하기
    5) 다 끝났으면 git rebase --continue
```

## git에서 관리하고 있지 않는 파일들 한번에 지우기 ===> git clean
```js
    c.f) 그냥 git clean 명령어를 치면 아무런 이벤트도 일어나지 않는다.
    
    1) git clearn -n ===> 삭제될 파일들 보여주기
    2) git clean -d ===> 폴더 포함
    3) git clean -df ===> (흔히 쓰이는 조합) 폴더포함 git 관리를 안하는 파일들 한꺼번에 지우기
```

## 특정파일을 특정 commit시점으로 되돌리려면? git restore --source

상황 : index.html 파일만 맨처음 시점으로 되돌리고 싶다.
```js   
    git restore --source='커밋해시' index.html
    c.f) 커밋을 한것은 아님 파일만 수정한 것임
    다시 원래 상태로 되돌아 가려면,,, ===> git restore index.html
```

## git reflog ===> 내가 그동안의 작업한 내역들을 확인하는 명령어
```js
    실수로 한 작업을 되돌리고 싶을 때
    git reflog를 쳐서 => git reset --hard "내가 실수로 한 커밋 이전 커밋해시"로 해결!
```

## 특정 커밋에다가 버전정보등을 달때 => git tag
tag의 종류 => 1) lightweight 2) annotated
```js
    상황 : v2.0.0이라는 태그를 달고 싶을때 ===> git tag v2.0.0 (lightweight)
    상황 : v2.0.0이라는 태그를 달고 싶을때(상세설명까지추가) ===> git tag -a v2.0.0 (annotated)

    git tag ===> tag의 리스트들 보기
    git show v2.0.0 ===> v2.0.0이라는 이름의 태그를가진 커밋내역 확인하기 (lightweight)
    git tag -d v2.0.0 ===> v2.0.0이라는 이름의 태그를 지우기 (lightweight)

    git tag v2.0.0 -m "자진모리장단" ===> v2.0.0이라는 태그를 달고 상세 설명에 "자진모리장단" 
    git tag v2.0.0 "커밋해시" -m "메세지" ===> 특정 "커밋해시"에 "v2.0.0"이라는 태그명을 달고, 
    그에 대한 상세내용은 "메세지"이다.
    git tag -l 'v1.*' ===> 태그의 이름이 "v1."으로 시작하는 모든 태그들을 본다.
    git tag -l '*0' ===> 태그의 이름이 "0"으로 끝나는 모든 태그들을 본다.
    git checkout v2.0.0 ===> HEAD가 v2.0.0의 태그로 checkout한다.
```

## merge의 두가지 방법 (fast-forward vs 3-way-merge)
1) fast-forward는 한가지 브랜치에서만 커밋내역이 있을때 HEAD만 제일 최신의 커밋끝단으로만 빨리 감기를 하는 것이다.
   단점) 어떤 브랜치와 병합했는지 기록이 남지 않는다.
         ===> 만약 브랜치의 병합 히스트로리를 남기고 싶다면, merge를 할때

```js
    git merge --no-ff "병합할 브랜치명" 으로 작성하면 된다. 
```

2) 3-way merge는 양쪽 브랜치 모두에서 하나 이상의 커밋이 있을때 새로운 병합 커밋을 새로 생성한다.
   ### 왜 3-way merge라고 할까? 
   만약 A 브랜치와 B 브랜치를 병합한다고 하자.
   두개의 브랜치를 병합하고자 할때, 각각의 브랜치에서 한번 이상의 커밋이 있을때 3-way merge를 하는데
   A브랜치 하나, B브랜치 하나, 그리고 A와 B의 공통조상이 되는 분기점 하나 그래서 3곳을 비교하여 
   어떤 파일이 수정되었는지, 충돌이 났는지 비교를 해야하기 때문에 3-way merge라고 하는것이다.

## 다른 브랜치에서 원하는 커밋만 따와서 적용하기 (cherry-pick)
```js
    git cherry-pick "특정커밋해시" ===> 특정커밋에 있는 변화들을 복제해서 가져오는 것이다. (그래서 cherry-pick으로 가져온 새로운 커밋과 그 대상이 되는 커밋은 별개의 커밋이다.)
```
## 다른 브랜치에서 파생된 브랜치 옮겨붙이기 (rebase --onto)
    특정 커밋을 가져올때 일일이 cherry-pick을 해도 되지만, 한꺼번에 가져오고 싶다면 rebase --onto 명령어를 사용해 보자.
    주의!) rebase --onto 명령어는 브랜치에서 또 파생된 브랜치를 가져올때 쓰는 명령어이다. 주의할것.
```js
    git rebase --onto (도착 브랜치) (출발 브랜치) (이동할 브랜치)
    c.f) 암기 TIP 나는 (도착 브랜치)로 (출발 브랜치)에 있는 (이동할 브랜치)를 옮겨 붙이겠다.
        그리고 rebase --onto는 해당하는 브랜치를 복사하는 것이 아니다. 그것을 그대로 옮기는것이다.

    rebase --onto가 완료되었다면, 일반적인 rebase방식으로 fast-forward를하자.
    ===> git switch master => git merge "rebase한 브랜치명"
```

## 다른 브랜치의 여러 커밋들을 하나로 묶어서 가져오기 (merge --squash)
```js
    git merge --squash (대상 브랜치) => 변경사항들이 스테이지 되어 있음 (git add 되어 있음) ===> 이후 본인 마음대로 수정및 commit하면된다.
```
![](./images/4.png)

## git diff (코드의 변경사항을 확인할 수 있는 명령어)

- working directory의 변경사항들을 확인하고 싶다면?
```js
    git diff 
```
- staging area 의 변경사항들을 확인하고 싶다면?
```js
    git diff --staged
```
- 간단하게 파일명만 보고싶다면,,, ===> --name-only
```js
    git diff --name-only 
    git diff --staged --name-only
```
- 커밋간의 차이 확인
```js
    git diff (커밋해시1) (커밋해시2)
    시간상 (커밋해시1)의 코드가 이전이고 (커밋해시2)의 코드가 이후이다.
```
- 브랜치간의 차이도 확인할 수 있다.
```js
    git diff (브랜치1) (브랜치2)
```

## git bisect (오류가 발생한 시점 찾아내기)
- git bisect란? 
  이진 탐색 알고리즘으로 오류가 나는 커밋의 시점을 찾아내는 git 명령어

- 순서
```js
    1) 'git bisect start' 로 bisect 명령어 시작
    2) 현재 오류가 나고 있으니, 'git bisect bad'로 현재 커밋이 오류가 난다고 찍어준다.
    3) 오류가 의심되는 지점으로 이동한다 => 'git checkout (오류가 의심되는 커밋해시)'
    4) 이 이후로 테스트를 하면서, 다음의 과정을 반복
       오류가 난다면 => 'git bisect bad'
       오류가 나지 않고 정상 작동한다면 => 'git bisect good'
    5) 그러면 git이 최초로 오류가 나기 시작한 커밋지점을 뱉어준다.
    6) bisect 과정을 종료하고 싶다면 => 'git bisect reset'
```


