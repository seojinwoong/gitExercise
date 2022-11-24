## 과거로 돌아가기 => RESET과 REVERT의 차이점

- RESET은 돌아가고자 하는 그 해시코드로 돌아간다 (기록이 남지 않는다.)
- REVERT는 되돌릴 해시코드를 지정한다 (기록이 남는다.) => 협업시에는 이것을 사용할것!


예를 들어보면,

```js
const totalCount = 53
const limit = 5

let totalPage = Math.ceil(totalCount / limit) // 11
```

#### 현재 페이지의 그룹 계산하기

![](https://user-images.githubusercontent.com/16531837/145596540-7c1ff5e6-60f8-40fc-884b-c10f4f4716a2.png)

### json-server

https://github.com/typicode/json-server
