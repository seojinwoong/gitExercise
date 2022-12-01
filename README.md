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
```
```js
    git remote 또는 git remote -v
    => 연결된 원격의 상세정보 알아보기.
```
```js
    git remote remove (origin 등 원격의 이름)
    => 원격 지우기 (로컬 프로젝트와의 연결만 없애는 것. GitHub의 레포지토리는 지워지지 않음)
```
## 내가 커밋한 것도 있고, 상대방이 커밋한 것도 있을때 (pull => push 순서)
```js
    1. pull(merge 방식) => git pull --no-rebase ===> 기록이 남는다.

    2. pull(rebase 방식) => git pull --rebase ===> 원격의 커밋을 먼저 붙이고 그다음 내 커밋을 뒤에다가 붙인다. 기록이 남지 않는다.
```


