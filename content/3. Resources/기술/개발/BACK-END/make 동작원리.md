## 요약
- makefile은 파일 간의 종속성과 규칙을 기반으로 동작한다.
```makefile
target: dependencies
	recipe
```
## 내용
### Make?
빌드 자동화 도구이다. Makefile에 작성된 규칙에 따라 소스 코드 파일을 컴파일하고 실행 파일을 생성하는 작업을 자동화할 수 있다.

make는 파일 간의 종속성과 규칙을 기반으로 작동하며 형식은 아래와 같다.
```makefile
target: dependencies
	recipe
```
- `target`: 생성하려는 파일 또는 작업 이름
- `dependencies`: target을 생성하기 위해 필요한 파일들이나 작업
- `recipe`: target을 생성하기 위해 실행 되어야 하는 명령어

### 동작 순서
1. Makefile 읽기
2. 규칙 종속성 확인: Makefile에서 첫 번째 규칙을 찾고 해당 규칙의 target과 그에 대한 종속성(dependencies)를 확인한다.
3. 종속성 체인 확인: 종속성 규칙을 확인하고 재귀적으로 반복한다.
4. 작업 실행: recipe에 따라서 진행
5. 파일 업데이트
6. 다른 규칙 실행: 첫 번째 규칙이 완료 된 후에도 Makefile에 다른 규칙이 정의되어 있다면 해당 규칙을 실행한다.
- + makefile은 파일 간의 종속성 그래프를 기반으로 작업을 실행하기 때문에 파일이 변경되지 않았다면 해당 파일에 대한 작업은 생략된다.

### 예시
```makefile
JS_FILES := main.js utils.js

app.js: $(JS_FILES)
    cat $(JS_FILES) > app.js

clean:
    rm -f app.js

```
JavaScript 파일을 하나의 파일로 결합하는데 사용하는 Makefile이다.
- `app.js` 규칙: app.js라는 target 파일을 생성하기 위해서 main.js와 utils.js가 필요하는 것을 명시한다. 규칙에는 파일을 결합하는 명령어인 cat이 사용된다.
- `clean` 규칙: clean이라는 target을 정의하여 생성된 파일을 제거하는데 사용된다. clean 규칙의 recipe에는 파일 삭제 명령어가 포함되어 있다.

## 참고
- [GNU make](https://www.gnu.org/software/make/manual/make.html)