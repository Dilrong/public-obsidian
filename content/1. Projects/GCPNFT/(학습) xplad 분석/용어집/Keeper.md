## 요약
- 코스모스 SDK의 모듈 데이터를 읽고 쓰는 기능을 제공하여 안전하게 호출할 수 있도록 도와준다.
## 내용
- 코스모스 SDK 블록체인 프레임워크에서 특정 모듈의 상태(state)를 관리하고 비즈니스 로직을 실행하는 핵심 컴포넌트이다.
- 모듈의 데이터를 읽고 쓰는 기능을 제공하며 다른 모듈이나 외부에서 모듈의 기능을 안전하게 호출할 수 있도록 도와준다.
### Keeper의 주요 역할
#### 1. 상태 관리
- Keeper의 특정 모듈의 데이터 저장소에 접근하여 해당 모듈의 상태를 저장하고 불러오는 역할을 한다.
- 예를 들면, `BankKeeper`는 사용자의 잔액 데이터를 관리하고 `StakingKeeper`는 스테이킹 관련 데이터를 저장하고 관리한다.
#### 2. 비즈니스 로직 수행
- Keeper는 모듈의 주요 비즈니스 로직을 구현한다.
- BankKeeper는 송금, 잔액 조회, 입출금 등의 기능을 수행한다.
- 각 기능은 조건을 만족할 때만 실행되며, 조건 검사를 통해 비즈니스 로직 무결성을 보장한다.
#### 3. 안전한 모듈 접근 제공
- 다른 모듈이나 외부에서 데이터를 안전하게 접근할 수 있는 인터페이스를 제공한다.
#### 4. 권한 제어
- 특정 작업에 대한 권한 검사를 수행한다. 
- 권한을 통해서 모듈 간의 데이터를 보호한다.
### BankDeeper 예시
```go
type BankKeeper struct {
	storeKey sdk.StoreKey
	cdc      codec.BinaryCodec
}
```
- 데이터 접근: `storeKey`를 통해 모듈의 상태에 접근하고, 사용자 계정과 잔액 정보를 관리한다.
- 비즈니스 로직
	- `SendCoins`: 사용자 간의 송금을 처리하며, 계정 간의 잔액 업데이트를 수행한다.
	- `GetBalance`: 특정 계정의 잔액을 조회한다.
	- `MintCoins`, `BurnCoins`: 코인 발행 및 소각 기능을 제공하여 토큰 공급을 조절한다.
- 안전한 외부 접근: 다른 모듈이나 애플리케이션에서는 Keeper의 함수를 호출하여 사용한다.
## 참고