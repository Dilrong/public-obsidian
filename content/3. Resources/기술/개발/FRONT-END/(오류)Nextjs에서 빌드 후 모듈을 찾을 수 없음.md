## 요약
- nodejs가 ES Module을 CommonJS 모듈 로더로 불러서 생긴 문제로 next.config에서 해당되는 패키지를 transpilePackages에 적용한다.

## 내용
- vote_web 프로젝트를 빌드하고 실행하면 아래와 같은 오류 발생
```zsh
Error [ERR_MODULE_NOT_FOUND]: Cannot find package 'axios' imported from C:\NFT_dev\xpla-vote\vote_web\.next\server\pages\_document.js
Did you mean to import axios-npm-1.5.1-6bc68e7d25-4444f06601.zip/node_modules/axios/dist/node/axios.cjs?
    at new NodeError (node:internal/errors:387:5)
    at packageResolve (node:internal/modules/esm/resolve:852:9)
    at moduleResolve (node:internal/modules/esm/resolve:901:20)
    at defaultResolve (node:internal/modules/esm/resolve:1115:11)
    at nextResolve (node:internal/modules/esm/loader:163:28)
    at ESMLoader.resolve (node:internal/modules/esm/loader:841:30)
    at ESMLoader.getModuleJob (node:internal/modules/esm/loader:424:18)
    at ESMLoader.import (node:internal/modules/esm/loader:525:22)
    at importModuleDynamically (node:internal/modules/cjs/loader:1129:29)
    at importModuleDynamicallyWrapper (node:internal/vm/module:438:21)
    at importModuleDynamically (node:vm:389:46)
    at importModuleDynamicallyCallback (node:internal/process/esm_loader:35:14)
    at Object.9648 (C:\NFT_dev\xpla-vote\vote_web\.next\server\pages\_document.js:162:1)
    at __webpack_require__ (C:\NFT_dev\xpla-vote\vote_web\.next\server\webpack-runtime.js:25:42)
    at C:\NFT_dev\xpla-vote\vote_web\.next\server\chunks\6184.js:17:63
    at Function.__webpack_require__.a (C:\NFT_dev\xpla-vote\vote_web\.next\server\webpack-runtime.js:89:13) {
  code: 'ERR_MODULE_NOT_FOUND'
```
- 해당 코드에서 오류가 난 부분을 보면 아래와 같음
```js
...
/***/ 997:
/***/ ((module) => {

module.exports = require("react/jsx-runtime");

/***/ }),

/***/ 9648:
/***/ ((module) => {

module.exports = import("axios");;

/***/ })
...
```
- axios의 경우 ESM 방식으로 사용하는데, webpack 설정을 ESM를 지원하지 않음으로 설정 한것으로 보인다.
- transpilePackages를 이용해서 nextjs에서 ESM로 사용 가능한 형태로 바꿔준다.
    - https://nextjs.org/docs/app/api-reference/next-config-js/transpilePackages
```js
/// next.config.js
const nextConfig = {
    ...
    transpilePackages: ['axios', 'redux-saga'],
}
```


## 참고
- [[CJS와 ESM]]
- [ESM 삽질기 - 카카오 스타일 블로그](https://devblog.kakaostyle.com/ko/2022-04-09-1-esm-problem/)
- [esm으로 번들링한 모듈을 ssr프레임워크에서 사용시 import 에러 해결법(SyntaxError: Cannot use import statement outside a module)](https://velog.io/@ssoon-m/esm%EC%9C%BC%EB%A1%9C-%EB%B2%88%EB%93%A4%EB%A7%81%ED%95%9C-%EB%AA%A8%EB%93%88%EC%9D%84-ssr%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%EC%8B%9C-import-%EC%97%90%EB%9F%ACSyntaxError-Cannot-use-import-statement-outside-a-module)