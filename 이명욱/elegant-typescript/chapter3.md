# 3장 고급 타입

## Never 타입
값을 반환할 수 없는 타입
- 에러를 던지는 경우
```js
function generateError(res: Response): never {
  throw new Error(res.getMessage());
}
```
- 무한히 함수가 실행되는 경우
```js
function checkStatus():never {
  while (true) {
    // ...
  }
}
```

> Object.prototype.toString.call(...) <- 객체의 타입을 알아내는 데 사용하는 함수
>typeof는 object 타입 까지만, 위의 Object...은 객체의 인스턴스까지 알려줌