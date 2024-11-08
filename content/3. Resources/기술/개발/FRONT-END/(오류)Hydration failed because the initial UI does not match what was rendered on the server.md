## 요약

## 내용
```zsh
Unhandled Runtime Error

Error: Hydration failed because the initial UI does not match what was rendered on the server.

Warning: Expected server HTML to contain a matching <table> in <div>.

See more info here: https://nextjs.org/docs/messages/react-hydration-error
```
무한 스크롤 작업에서 위와 같은 오류 발생

### 해결
- `<tr></tr>`을 사용해서 생긴 문제로 `<table/>` 태그는 예전꺼라서 그런지 문제가 있는듯함

## 참고
- https://velog.io/@j1min/Next.js-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%BD%94%EB%94%A9%EC%8B%9C-hydration-error-%ED%95%B4%EA%B2%B0%EB%B2%95