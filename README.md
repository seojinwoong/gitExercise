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
## 병합하는 두가지 방법 : merge vs rebase

- merge : 브랜치의 이력이 남는다.
- rebase : 브랜치의 이력이 남지 않아 history가 깔끔해지지만, 협업하는 과정에서는 이 방법은 지양하는 것이 좋다.

#### 현재 페이지의 그룹 계산하기

![](https://user-images.githubusercontent.com/16531837/145596540-7c1ff5e6-60f8-40fc-884b-c10f4f4716a2.png)

### json-server

https://github.com/typicode/json-server
