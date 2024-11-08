## 요약

## 내용
```zsh
ESLint: Expected linebreaks to be 'CRLF' but found 'LF'.(linebreak-style)
```
- 웹스톰 IDE 업데이트 이후에 저장하면 위와 같이 오류가 발생
- fix current file로 수동으로 눌러주면 일시적으로는 해결되지만 작업 후 저장하면 다시 위와 같이 오류 발생

### 방법1. eslintrc.js 수정
```js
module.exports = {
    rules: {
        'linebreak-style': 'off',
    },
};
```
- 옵션을 꺼버리는 방식으로 같이 사용하는 프로젝트에서는 당연히 하면 안됨
```js
        'linebreak-style': [
            'error',
            require('os').EOL === '\r\n' ? 'windows' : 'unix',
        ],
```
애초에 위에 처럼 되어 있는데, 문제가 있음

### 방법2. 에디터 라인을 CRLF에서 LF로 변경하기
- Editor > Code Style >  Line separator를 Unix and macOS로 변경하여 강제로 LF로 통일한다.

## 참고