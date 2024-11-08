
## 내용
### 설정 우선순위
- `settings.json < .editorconfig < .prettierrc`

### editorconfig
- 다른 에디터나 IDE를 사용해도 일관된 코딩 스타일을 유지하도록 도와주는 도구이다.

### ESlint
- JavaScript 코드를 일관된 코딩 스타일을 유지하도록 도와주는 도구이다.
- 설정은 프로젝트 루트에 `.eslintrc`을 만들어서 사용한다.
- 다양한 파일을 지원하여 뒤에 원하는 확장자만 붙여주면 된다. `.eslintrc.js`, `.eslintrc.json`...
- ESLint는 크게 4가지의 설정을 지원한다.
    - 환경(Env): 스크립트가 실행되도록 설계된 환경이다.
    - 전역(Globals): 스크립트가 실행 중에 접근하는 추가 전역 변수이다.
    - 규칙(Rules): 활성화된 규칙과 오류 수준
        - "off": 규칙 비활성화
        - "warn": 경고로 설정
        - "error": 오류로 설정
    - 플러그인(Plugins): 타사 플러그인
- https://eslint.org/docs/latest/use/configure/에서 옵션을 확인 할 수 있다.

### prettier
- 코드 포맷터로 정해진 코딩 스타일을 따르도록 자동으로 변환해주는 도구이다.
- 설정은 프로젝트 루트에 `.prettierrc`을 만들어서 사용한다.
- 설정을 무시할 파일은 프로젝트 루트에 `.prettierignore`을 만들어서 사용한다.
- https://prettier.io/docs/en/options.html 에서 옵션을 확인 할 수 있다.

### husky
- 커밋할 때 린트, 테스트 작업을 해주는 도구이다.
  
## 참고
- https://editorconfig.org/
- https://eslint.org
- https://prettier.io
- https://typicode.github.io/husky/