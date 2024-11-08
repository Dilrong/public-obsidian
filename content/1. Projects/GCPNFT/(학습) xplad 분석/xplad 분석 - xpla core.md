## 요약

## 내용
### 구조
- .github
- ante: AnteHandler (트랜잭션 처리)
- app: 핵심 로직 및 초기 설정
- cmd: CLI
	- [[xplad cmd function]]
- docs: Swagger (API 문서)
- proto: 직렬화 데이터 통신
- scripts: 스크립트
- tests: 테스트코드
- third_party
- types: 타입
- x: xpla 고유 모듈 정의
### 주요 라이브러리
- [[Cobra]]: CLI의 Command 구현을 지원 (cmd)
- [[Viper]]: 설정 관리 모듈
- [[Gorilla]]: was 라이브러리
- [[CometBFT]]: 네트워크, 합의 레이어
- [[CosmWasm]]: 스마트 컨트랙트 모듈
- [[Ethermint]]: 이더리움 Tx 지원 모듈
### 디자인패턴
- Modular Design: 서비스 단위를 모듈로 분리하고 필요한 모듈을 연결해서 사용하는 형식 (cosmos-sdk의 모듈을 사용한다.)
## 참고
- https://github.com/xpladev/xpla