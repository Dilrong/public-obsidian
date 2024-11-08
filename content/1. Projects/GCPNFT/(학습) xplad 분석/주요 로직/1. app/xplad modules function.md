## 요약
- XPLA에서 사용하는 코스모스 SDK 및 커스텀 모듈 구성 및 초기화를 한다. - [[Cosmos Moduler]]
## 내용
- XPLA 블록체인 모듈 구성 및 초기화 과정을 정의한다.
### 1. `maccPerms`
- 모듈 계정의 권한을 정의하는 맵이다. 각 모듈이 어떤 권한을 가지는지 지정하여 모듈이 특정 작업을 수행할 수 있도록 한다.
```go
var maccPerms = map[string][]string{  
    authtypes.FeeCollectorName:     nil,  
    distrtypes.ModuleName:          nil,  
    icatypes.ModuleName:            nil,  
    minttypes.ModuleName:           {authtypes.Minter},  
    stakingtypes.BondedPoolName:    {authtypes.Burner, authtypes.Staking},  
    stakingtypes.NotBondedPoolName: {authtypes.Burner, authtypes.Staking},  
    govtypes.ModuleName:            {authtypes.Burner},  
    ibctransfertypes.ModuleName:    {authtypes.Minter, authtypes.Burner},  
    ibcfeetypes.ModuleName:         nil,  
    wasmtypes.ModuleName:           {authtypes.Burner},  
    evmtypes.ModuleName:            {authtypes.Minter, authtypes.Burner},
    erc20types.ModuleName:          {authtypes.Minter, authtypes.Burner},  
    rewardtypes.ModuleName:         nil,  
}
```
- 예시
	- `minttypes.ModuleName`은 `{authtypes.Minter}` 권한을 가지고 있어 토큰을 발행할 수 있다.
	- `evmtypes.ModuleName`과 `erc20types.ModuleName`은 `{authtypes.Minter, authtypes.Burner}` 권한을 가지며, 토큰 발행과 소각이 가능하다.
### 2. `getGovProposalHandlers`
- 거버넌스 모듈이 처리할 수 있는 제안 핸들러를 정의한다. 각 핸들러는 특정 제안 유형에 대해 동작하도록 구성된다.
- 주요 제한 핸들러는 다음과 같다.
	- `upgradeclient.LegacyProposalHandler`: 네트워크 업그레이드 제안
	- `ibcclientclient.UpdateClientProposalHandler`: IBC 클라이언트 업데이트 제안
	- `erc20client.RegisterCoinProposalHandler`: 새로운 ERC20 토큰 등록 제안
```go
func getGovProposalHandlers() []govclient.ProposalHandler {  
    govProposalHandlers := volunteerclient.ProposalHandler  
  
    return append(govProposalHandlers, []govclient.ProposalHandler{  
       upgradeclient.LegacyProposalHandler,  
       upgradeclient.LegacyCancelProposalHandler,  
       ibcclientclient.UpdateClientProposalHandler,  
       ibcclientclient.UpgradeProposalHandler,  
       erc20client.RegisterCoinProposalHandler,  
       erc20client.RegisterERC20ProposalHandler,  
       erc20client.ToggleTokenConversionProposalHandler,  
    }...)  
}
```
### 3. `ModuleBasics`
- 블록체인의 모든 모듈에 대한 기본 설정을 관리한다. 기본 초기화 설정을 정의하고, 모듈 간 의존성 처리를 한다.
```go
var ModuleBasics = module.NewBasicManager(  
    auth.AppModuleBasic{},  
    genutil.NewAppModuleBasic(genutiltypes.DefaultMessageValidator),  
    bank.AppModuleBasic{},  
    capability.AppModuleBasic{},  
    staking.AppModuleBasic{},  
    mint.AppModuleBasic{},  
    distr.AppModuleBasic{},  
    gov.NewAppModuleBasic(getGovProposalHandlers()),  
    params.AppModuleBasic{},  
    crisis.AppModuleBasic{},  
    slashing.AppModuleBasic{},  
    feegrantmodule.AppModuleBasic{},  
    authzmodule.AppModuleBasic{},  
    consensus.AppModuleBasic{},  
    ibc.AppModuleBasic{},  
    ibctm.AppModuleBasic{},  
    ibcfee.AppModuleBasic{},  
    upgrade.AppModuleBasic{},  
    evidence.AppModuleBasic{},  
    transfer.AppModuleBasic{},  
    router.AppModuleBasic{},  
    ica.AppModuleBasic{},  
    erc20.AppModuleBasic{},  
    wasm.AppModuleBasic{},  
    evm.AppModuleBasic{},  
    feemarket.AppModuleBasic{},  
    reward.AppModuleBasic{},  
    volunteer.AppModuleBasic{},  
)
```
### 4. `appModules`
- 각 모듈을 초기화하고 **`module.AppModule` 인터페이스를 구현**하여 블록체인에서 사용할 수 있도록 한다.
- 주요 모듈 설정
	- `bank.NewAppModule`: 은행 모듈을 초기화하여 송금, 잔액 조회 등의 기능을 제공한다.
	- `evm.NewAppModule`: EVM 모듈을 통해 이더리움 호환 기능을 지원한다.
	- `wasm.NewAppModule`: WASM 모듈을 통해 스마트 컨트랙트를 관리할 수 있게 한다.
	- `erc20.NewAppModule`: ERC20 모듈을 추가하여 토큰 변환과 같은 기능을 지원한다.
### 5. `orderBeginBlockers`, `orderEndBlockers`, `orderInitBlocker`
- 모듈의 `BeginBlocker`, `EndBlocker`, `InitGenesis` 실행 순서를 정의 한다.
	-  `BeginBlockers`: 블록 시작 시 실행되는 함수들로, 업그레이드, 상태 초기화 등을 담당한다.
	- `EndBlockers`: 블록 종료 시 실행되는 함수들로, 보상 분배, 스테이킹 업데이트 등을 수행한다.
	- `InitBlockers`: 체인이 처음 시작될 때 제네시스 데이터를 초기화하고 상태를 설정한다.
### 전체 흐름
1. 모듈 권한 설정 - `maccPerms`
2. 거버넌스 제안 핸들러 설정 - `getGovProposalHandlers`
3. 모듈 초기화 - `ModuleBasics`, `appModules`
4. 실행 순서 정의 - `orderBeginBlockers`, `orderEndBlockers`, `orderInitBlockers`
## 참고