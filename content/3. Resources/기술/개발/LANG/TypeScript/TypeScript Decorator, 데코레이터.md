## 요약

## 내용
- Typescript or ES6에 클래스 및 클래스 멤버에 [[(참고) 메타 프로그래밍]] 구문을 추가할 수 있는 방법이다. 특수한 종류의 선언으로 `@expression` 형식을 사용한다.
- 예를 들면 `@sealed`를 사용하면 다음과 같이 `sealed`함수를 작성할 수 있다.
```ts
function sealed(target) {
    // 'target' 변수와 함께 무언가를 수행합니다.
}
```
- 데코레이터 팩토리를 이용하여 데코레이터가 선언에 적용되는 방식을 원하는 대로 바꿀 수 있다.
	- 데코레이터 팩토리는 단순히 데코레이터가 런타임에 호출할 표현식을 반환하는 함수이다.
```ts
function color(value: string) { // 데코레이터 팩토리
    return function (target) { // 데코레이터
        // 'target'과 'value' 변수를 가지고 무언가를 수행합니다.
  };
}
```
- `reflect-meta`는 자바스크립트의 [Reflect](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect)객체를 활용하여 클래스 또는 함수에 메타데이터를 추가하거나 수정할 수 있게 해준다.
	- Reflect는 중간에서 가로챌 수 있는 자바스크립트 작업에 대한 메서드를 제공하는 내장 객체이다.
	- 메타 프로그래밍할 때 사용한다.
## 참고
- https://www.typescriptlang.org/ko/docs/handbook/decorators.html
- https://www.npmjs.com/package/reflect-metadata