# Hoisting

`var` 로 선언된 변수는 코드가 실행되기 전에 선언 처리하기 때문에, 코드 안에서 최상위 단에 선언한 것과 동일하다.

```javascript
// case A
a = 1; // 아직 선언되지 않았지만 바로 아래 선언되었기 때문에 B와 같이 동작한다.
var a;

// case B
var a;
a = 1;
```
