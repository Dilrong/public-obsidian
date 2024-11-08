## 요약
- NextJs는 자바스크립트 실행 전에 HTML을 생성하기 때문에 데이터 종속성 관리를 신경써야한다.
## 내용
- NextJs는 모든 페이지를 미리 렌더링 한다.
- 이는 자바스크립트 실행 전에 HTML 생성한다는 것을 의미한다.
- 이를 Pre-Rendering이라고 한다.
	- Pre-Rendering을 사용하지 않는 경우 바로 Hydration과정을 거친다.![[no-pre-rendering.png]]
	- Pre-Rendering을 사용하는 경우 HTML을 표시하고 JS를 추가한다.![[pre-rendering.png]]
- Pre-Rendering은 정적 생성(Static Generation), 서버 사이드 렌더링(Server-side Rendering) 두 가지 형태가 있다.
- 데이터 종속성이 존재하는 경우(페이지 렌더링에서 필요한 데이터가 있는 경우) `getStaticProps()` 또는 `getServerSideProps()`를 사용해야 한다.
	- `getStaticProps()`는 최초 생성 이후로 데이터 업데이트가 필요 없는 페이지에서 사용한다. (빌드 시에 딱 한번만 호출된다.)
	- `getServerSideProps()`는 데이터 업데이트가 이루어지는 페이지에서 사용한다. (페이지가 요청받을 때 마다 호출된다.)

## 참고
- https://nextjs.org/learn-pages-router/basics/data-fetching/pre-rendering
- https://wonit.tistory.com/362
- https://velog.io/@rdt419/Redux-toolkit-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%ACfeat.-Next-Js