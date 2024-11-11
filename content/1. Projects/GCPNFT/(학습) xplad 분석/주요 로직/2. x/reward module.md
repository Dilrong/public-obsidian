## 요약
- 검증인(Validator)들에게 보상을 분배하는 기능을 담당하는 XPLA 커스텀 모듈
## 내용
- 검증인(Validator)들에게 보상을 분배하는 기능을 담당한다.
- Cosmos SDK의 모듈 구조를 따른다.
### 폴더 구조
- `keeper`
	- 핵심 비즈니스 로직을 구현하는 파일이 위치한다.
		- [[Keeper]]참고
- `types`
	- 모듈에서 사용하는 데이터 구조, 메시지, 이벤트 타입을 정의한다.
- `module.go`
	- Cosmos SDK 모듈 인터페이스를 구현한다.
	- 모듈 초기화, Genesis 상태 정의, BeginBlock과 EndBlock 로직 등이 포함된다.
- `abci.go`
	- BeginBlock과 EndBlock에서 트리거되는 로직을 포함한다. 보상 분배 로직이 이 파일에 포함된다.
- `genesis`
	- 모듈의 초기 상태를 설정하고, Genesis 파일에서 가져올 초기 설정과 관련된 내용을 포함한다.
## 참고