## 요약
- Node12버전 이전의 모듈 시스템은 CommonJS(CJS), 이후에는 ECMAScript Modules(ESM)라는 두 가지 모듈 시스템이 존재한다.
## 내용
Node.js 12버전부터 ECMAScript Modules라는 새로운 Module System이 추가되면서, 기존 CommonJS라는 Module System까지, 라이브러리는 두 가지 Module System을 지원해야한다.
- CommonJS(CJS)
```js
// add.js
module.exports.add = (x, y) => x + y;

// main.js
const { add } = require('./add');

add(1, 2);
```
- ECMAScript Modules (ESM)
```js
// add.js
export function add(x, y) {
  return x + y
}

// main.js
import { add } from './add.js';

add(1, 2);
```
- CJS는 `require`/`module.exports`를 사용하고 ESM은 `import`/`export`문을 사용한다.
- module loader는 CJS는 동기적으로 ESM는 비동기적으로 작동한다.
    - Top-level await를 지원히기 때문이다. (모듈의 최상위 스코프에서 비동기 동작은 await 하여 사용하는 것, 모듈의 동작을 await 할 수 있다.)
## 참고
- [CommonJS와 ESM에 모두 대응하는 라이브러리 개발하기: exports field](https://toss.tech/article/commonjs-esm-exports-field)