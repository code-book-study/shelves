# 2장 타입

## 타입을 확인하는 방법
타입스크립트에는 값 공간과 타입 공간이 별도로 존재

```js
// 값에 사용된 typeof는 자바스크립트 런타임의 typeof 연산자
const v1 = typeof person; // 값은 'object'

interface Person = {
  name: string;
}
// 타입에서 사용된 typeof는 값을 읽고 타입스크립트 타입 반환
cons person:Person = {name: "철수"}
type T1 = type of person; // 타입은 Person
```