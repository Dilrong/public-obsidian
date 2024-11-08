## 요약
- XPLA 블록체인 초기화 및 설정을 담당하는 코드로 `XplaApp` 구조체를 생성하고 설정한다.
## 내용
- XPLA 블록체인 초기화 및 설정을 담당한다.
- 코스모스 SDK를 기반으로 애플리케이션의 핵심 구성 요소 및 모듈을 초기화한다.
### `XplaApp` 구조체
- 주요 데이터 구조로 XPLA의 기본 기능 구현
	- `BaseApp`: 코스모스 SDK의 기본 애플리케이션 구조로 트랜잭션 실행, 상태 관리 등의 핵심 기능 제공한다.
	- `AppKeepers`: 모듈 데이터 관리하는 [[Keeper]]가 포함된다.
	- `legacyAmino` 및 `appCodec`: 데이터 인코딩 및 디코딩을 위한 코덱을 설정한다.
	- `txConfig`, `interfaceRegistry`: 트랜잭션 설정 및 인터페이스 레지스트리를 정의한다.
	- `mm`: `module.Manager`로, 애플리케이션의 모듈을 관리한다.
	- `configurator`: 모듈 설정 및 서비스 등록을 위한 `module.Configurator`를 포함한다.
```go
type XplaApp struct {
    *baseapp.BaseApp  
    keepers.AppKeepers  
  
    legacyAmino       *codec.LegacyAmino  
    appCodec          codec.Codec  
    txConfig          client.TxConfig  
    interfaceRegistry types.InterfaceRegistry  
  
    invCheckPeriod uint  
  
    // the module manager  
    mm *module.Manager  
  
    configurator module.Configurator  
}
```
### `NewXplaApp`
- XPLA 애플리케이션 인스턴스를 초기화하는 함수로 주요 모듈과 설정을 초기화한다.
- 단계
	- `BaseApp` 초기화: 트랜잭션 인코딩/디코딩 설정 및 커밋 트레이서를 설정한다.
	- `AppKeepers` 초기화: `keepers.NewAppKeeper`를 호출하여 앱의 모듈을 관리하는 Keeper를 초기화한다.
	- `Module Manager` 초기화: 모듈을 관리하는 `module.Manager`를 생성하고, `BeginBlockers`, `EndBlockers`, `InitGenesis` 순서를 설정한다.
	- 업그레이드 핸들러 설정: 업그레이드 시 필요한 모듈 설정을 위해 업그레이드 핸들를 설정한다.
	- AnteHandler 설정: 트랜잭션 실행 전 검증 작업을 수행하는 [[anteHandler]]를 초기화한다.
	- API 및 서비스 설정: gRPC, REST API 라우트, 텐더민트 서비스, 트랜잭션 서비스 등을 등록한다.
### `ModuleAccountAddrs`
- 애플리케이션의 모든 계정 주소를 반환한다.
- 모듈은 고유한 계정 주소를 가지며, 모듈이 계정을 통해 자산을 관리할 때 사용된다.
### `BlockedModuleAccountAddrs`
- 특정 모듈 계정이 송금할 수 없도록 차단한다.
### `BeginBlocker`, `EndBlocker` 함수
- `BeginBlocker`: 블록 시작 지점에서 실행되는 코드로 검증자 업데이트, 슬래싱 등의 작업 처리를 한다.
- `EndBlocker`: 블록 끝에서 실행되며, 스테이킹 보상 분배 작업 처리를 한다.
### `InitChainer`
- 체인이 초기화될 때 제네시스 상태 설정을 한다.
### `setUpgradeStoreLoaders`, `setUpgradeHandlers` 
- 업그레이드될 때 적용해야 하는 설정을 정의한다.
- `setUpgradeStoreLoaders`: 특정 블록 높이에 도달하면 업그레이드를 트리거하고, 필요한 스토어 로더를 설정한다.
- `setUpgradeHandlers`: 업그레이드 시 필요한 모듈 설정을 업데이트하는 핸들러를 설정한다.
### `RegisterAPIRoutes`
- REST API 라우터 설정을 한다.
### `noOpTxFeeChecker`
- TxFeeChecker로 트랜잭션 수수료를 확인하지 않고 항상 0을 반환하는 단순한 함수이다.
- 주로 테스트 용도로 사용하고, 트랜잭션 수수료가 필요 없는 경우 유용하다.
## 참고