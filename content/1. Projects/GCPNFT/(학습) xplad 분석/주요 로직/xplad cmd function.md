## 요약
- xpla CLI 설정에 대한 함수를 정의한다. 
- `xplad 커맨드 인자`형식으로 사용된다.
## 내용
- xplad CLI에 해당하는 함수 정의
### cmd/xplad/main.go
- 주소 prefix 설정, 코인 타입, 데몬() 설정
```go
func main() {  
    // Set address prefix and cointype  
    types.SetConfig()  
  
    err := sdk.RegisterDenom(types.DefaultDenom, sdk.NewDecWithPrec(1, types.DefaultDenomPrecision))  
    if err != nil {  
       panic(err)  
    }  
  
    rootCmd, _ := cmd.NewRootCmd()  
  
    if err := svrcmd.Execute(rootCmd, "", app.DefaultNodeHome); err != nil {  
       switch e := err.(type) {  
       case server.ErrorCode:  
          os.Exit(e.Code)  
  
       default:  
          os.Exit(1)  
       }  
    }  
}
```
### cmd/xplad/cmd/root.go
#### `NewRootCmd`
- XPLA CLI 루트 명령어 생성을 한다.
#### `initTendermintConfig`
- 텐더민트 기본 설정에 오버라이드를 한다.
#### `initAppConfig`
- XPLA의 애플리케이션 레벨 설정 초기화
	- `StateSync`: 스냅샷 생성과 유지에 대해서 설정한다.
	- `bypassMinFeeMsgTypes`: 특정 IBC 관련 메시지는 최소 수수료 설정 우회하도록 설정한다.
#### `initRootCmd`
- 루트 명령어에 하위 명령어 추가
	- Genesis 관련 명령어
	- RPC 및 Query 관련 명령어
#### `addModuleInitFlags`
- CLI에서 사용할 수 있는 플래그를 설정한다.
#### queryCommand
- XPLA 블록체인 데이터를 조회하는 명령어를 정의한다.
#### `txCommand`
- 트랜잭션 관련 명령어를 모은 tx 명령어를 정의한다.
#### `appCreator`
- `appCreator`구조체는 애플리케이션을 생성하고 내보내는 역할을 담당하는 구조체이다.
	- `newApp`: 새로운 XPLA 애플리케이션 인스턴스를 생성하는 함수로 애플리케이션 동작 방식을 설정한다.
	- `appExport`: 애플리케이션 현재 상태와 검증자 세트를 내보내는 함수로 재시작 및 백업을 위한 기능이다.
### cmd/xplad/cm
- d/genaccounts.go
- 초기 계정(GenesisAccount)를 생성한다.
- 계정 주소 또는 [[Keyring]], 키 이름, 초기 코인을 입력 받는다.
- xpla는 evm을 호환하기 위해서 ethermint를 사용하여 EthAccount를 기반으로 생성된다.
```go
func AddGenesisAccountCmd(defaultNodeHome string) *cobra.Command {  
    cmd := &cobra.Command{  
       Use:   "add-genesis-account [address_or_key_name] [coin][,[coin]]",  
       Short: "Add a genesis account to genesis.json",  
       Long: `Add a genesis account to genesis.json. The provided account must specify  
the account address or key name and a list of initial coins. If a key name is given,  
the address will be looked up in the local Keybase. The list of initial tokens must  
contain valid denominations.  
`,  
       Args: cobra.ExactArgs(2),  
       RunE: func(cmd *cobra.Command, args []string) error {  
          clientCtx := client.GetClientContextFromCmd(cmd)  
          serverCtx := server.GetServerContextFromCmd(cmd)  
          config := serverCtx.Config  
  
          config.SetRoot(clientCtx.HomeDir)  
  
          var kr keyring.Keyring  
          addr, err := sdk.AccAddressFromBech32(args[0])  
          if err != nil {  
             inBuf := bufio.NewReader(cmd.InOrStdin())  
             keyringBackend, err := cmd.Flags().GetString(flags.FlagKeyringBackend)  
             if err != nil {  
                return err  
             }  
  
             if keyringBackend != "" && clientCtx.Keyring == nil {  
                var err error  
                kr, err = keyring.New(  
                   sdk.KeyringServiceName(),  
                   keyringBackend,  
                   clientCtx.HomeDir,  
                   inBuf,  
                   clientCtx.Codec,  
                   hd.EthSecp256k1Option(),  
                )  
                if err != nil {  
                   return err  
                }  
             } else {  
                kr = clientCtx.Keyring  
             }  
  
             k, err := kr.Key(args[0])  
             if err != nil {  
                return fmt.Errorf("failed to get address from Keyring: %w", err)  
             }  
  
             addr, err = k.GetAddress()  
             if err != nil {  
                return fmt.Errorf("failed to get address from Keyring: %w", err)  
             }  
          }  
  
          coins, err := sdk.ParseCoinsNormalized(args[1])  
          if err != nil {  
             return fmt.Errorf("failed to parse coins: %w", err)  
          }  
  
          // create concrete account type based on input parameters  
          var genAccount authtypes.GenesisAccount  
  
          balances := banktypes.Balance{Address: addr.String(), Coins: coins.Sort()}  
          baseAccount := authtypes.NewBaseAccount(addr, nil, 0, 0)  
  
          genAccount = &ethermint.EthAccount{  
             BaseAccount: baseAccount,  
             CodeHash:    common.BytesToHash(evmtypes.EmptyCodeHash).Hex(),  
          }  
  
          if err := genAccount.Validate(); err != nil {  
             return fmt.Errorf("failed to validate new genesis account: %w", err)  
          }  
  
          genFile := config.GenesisFile()  
          appState, genDoc, err := genutiltypes.GenesisStateFromGenFile(genFile)  
          if err != nil {  
             return fmt.Errorf("failed to unmarshal genesis state: %w", err)  
          }  
  
          authGenState := authtypes.GetGenesisStateFromAppState(clientCtx.Codec, appState)  
  
          accs, err := authtypes.UnpackAccounts(authGenState.Accounts)  
          if err != nil {  
             return fmt.Errorf("failed to get accounts from any: %w", err)  
          }  
  
          if accs.Contains(addr) {  
             return fmt.Errorf("cannot add account at existing address %s", addr)  
          }  
  
          // Add the new account to the set of genesis accounts and sanitize the  
          // accounts afterwards.          accs = append(accs, genAccount)  
          accs = authtypes.SanitizeGenesisAccounts(accs)  
  
          genAccs, err := authtypes.PackAccounts(accs)  
          if err != nil {  
             return fmt.Errorf("failed to convert accounts into any's: %w", err)  
          }  
          authGenState.Accounts = genAccs  
  
          authGenStateBz, err := clientCtx.Codec.MarshalJSON(&authGenState)  
          if err != nil {  
             return fmt.Errorf("failed to marshal auth genesis state: %w", err)  
          }  
  
          appState[authtypes.ModuleName] = authGenStateBz  
  
          bankGenState := banktypes.GetGenesisStateFromAppState(clientCtx.Codec, appState)  
          bankGenState.Balances = append(bankGenState.Balances, balances)  
          bankGenState.Balances = banktypes.SanitizeGenesisBalances(bankGenState.Balances)  
          bankGenState.Supply = bankGenState.Supply.Add(balances.Coins...)  
  
          bankGenStateBz, err := clientCtx.Codec.MarshalJSON(bankGenState)  
          if err != nil {  
             return fmt.Errorf("failed to marshal bank genesis state: %w", err)  
          }  
  
          appState[banktypes.ModuleName] = bankGenStateBz  
  
          appStateJSON, err := json.Marshal(appState)  
          if err != nil {  
             return fmt.Errorf("failed to marshal application genesis state: %w", err)  
          }  
  
          genDoc.AppState = appStateJSON  
          return genutil.ExportGenesisFile(genDoc, genFile)  
       },  
    }  
  
    cmd.Flags().String(flags.FlagHome, defaultNodeHome, "The application home directory")  
    cmd.Flags().String(flags.FlagKeyringBackend, flags.DefaultKeyringBackend, "Select keyring's backend (os|file|kwallet|pass|test)")  
    flags.AddQueryFlagsToCmd(cmd)  
  
    return cmd  
}
```
## 참고
- https://github.com/xpladev/xpla/tree/main/cmd/xplad