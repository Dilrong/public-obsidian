## 요약

## 내용
### keepers
- 각 모듈의 Keeper 구조체를 정의하고 관리한다.
```go
type AppKeepers struct {
	// keys to access the substores
    map[string]*storetypes.KVStoreKey  
    tkeys   map[string]*storetypes.TransientStoreKey  
    memKeys map[string]*storetypes.MemoryStoreKey  
  
    // keepers  
    AccountKeeper    ethermintauthkeeper.AccountKeeper  
    BankKeeper       bankkeeper.Keeper  
    CapabilityKeeper *capabilitykeeper.Keeper  
    StakingKeeper    *xplastakingkeeper.Keeper  
    SlashingKeeper   slashingkeeper.Keeper  
    MintKeeper       mintkeeper.Keeper  
    DistrKeeper      distrkeeper.Keeper  
    GovKeeper        *govkeeper.Keeper  
    CrisisKeeper     *crisiskeeper.Keeper  
    UpgradeKeeper    *upgradekeeper.Keeper  
    ParamsKeeper     paramskeeper.Keeper  
    // IBC Keeper must be a pointer in the app, so we can SetRouter on it correctly  
    IBCKeeper             *ibckeeper.Keeper  
    IBCFeeKeeper          ibcfeekeeper.Keeper  
    ICAControllerKeeper   icacontrollerkeeper.Keeper  
    ICAHostKeeper         icahostkeeper.Keeper  
    EvidenceKeeper        evidencekeeper.Keeper  
    TransferKeeper        ibctransferkeeper.Keeper  
    FeeGrantKeeper        feegrantkeeper.Keeper  
    AuthzKeeper           authzkeeper.Keeper  
    ConsensusParamsKeeper consensusparamkeeper.Keeper  
  
    PFMRouterKeeper *pfmrouterkeeper.Keeper  
  
    RewardKeeper    rewardkeeper.Keeper  
    VolunteerKeeper volunteerkeeper.Keeper  
  
    // make scoped keepers public for test purposes  
    ScopedIBCKeeper           capabilitykeeper.ScopedKeeper  
    ScopedICAControllerKeeper capabilitykeeper.ScopedKeeper  
    ScopedFeeMockKeeper       capabilitykeeper.ScopedKeeper  
    ScopedTransferKeeper      capabilitykeeper.ScopedKeeper  
    ScopedICAHostKeeper       capabilitykeeper.ScopedKeeper  
  
    WasmKeeper       wasmkeeper.Keeper  
    scopedWasmKeeper capabilitykeeper.ScopedKeeper  
  
    EvmKeeper       *evmkeeper.Keeper  
    FeeMarketKeeper feemarketkeeper.Keeper  
    Erc20Keeper     erc20keeper.Keeper  
}
```
### params
- 전역 설정 값과 파라미터를 정의하고 관리한다.
### upgrades
- 네트워크 업그레이드에 관련된 설정 및 로직을 포함한다.
	- `const.go`: 변수를 설정한다.
	- `upgrades.go`: 업그레이드를 설정한다.

## 참고