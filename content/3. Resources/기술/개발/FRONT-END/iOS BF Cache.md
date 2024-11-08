## 요약
- iOS에서는 페이지이동에서 BFCache를 사용하는데, 이는 자바스크립트를 포함하여 페이지 전체를 캐시로 저장하고 그 페이지를 보여주기 때문에 자바스크립트가 실행되지 않음

## 내용
### 문제
- pc에서는 동작하지만 ios에서 리덕스와 router를 사용하는 동작인 로그아웃 기능이 동작이 없는 문제가 발생

### 해결
- dispatch와 router 함수를 같이 넣었는데, 이를 useEffect로 dispatch의 결과로 동작하도록 변경

### 원인
- ios에서는 뒤로가기 또는 동일 페이지로 이동하는 경우 BF Cache를 사용
- BFCache란 뒤로가기, 앞으로가기 눌렀을 때 화면을 바로 보여주는 역활
- BFCache는 자바스크립트를 포함하여 페이지 전체를 캐시로 저장해버리는 기능을 가짐
- 고로 자바스크립트가 다시 로드되어 동작을 해야하는 페이지에서는 문제가 발생

### 방안
- meta태그에서 캐싱을 비활성화
- window.onpageshow로 확인하고 실행
- 새로고침으로 자바스크립트 재로딩

## 참고
- https://goddaehee.tistory.com/266
- https://medium.com/teamo2/%EC%9E%A1%EC%95%98%EB%8B%A4-%EC%9A%94%EB%86%88-back-forward-%EC%BA%90%EC%8B%9C-b14a7cd35dd8