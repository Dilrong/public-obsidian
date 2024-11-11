## 요약

## 내용
### address.go
- 주소의 Prefix를 정의한다.
```go
const (  
    // Bech32MainPrefix defines the main SDK Bech32 prefix of an account's address
    Bech32MainPrefix = "xpla"
    ...
    // Bech32PrefixValAddr defines the Bech32 prefix of a validator's operator address
    Bech32PrefixValAddr = Bech32MainPrefix + PrefixValidator + PrefixOperator
    ...
```
### cointype.go
- 블록체인에서 인식하는 네이티브 코인 정의를 한다.
```go
package types  
  
const (  
    // Default Denom  
    DefaultDenom = "axpla"  
  
    // CoinType  
    CoinType = 60  
  
    // FullFundraiserPath is the parts of the BIP44 HD path
    FullFundraiserPath = "m/44'/60'/0'/0/0"  
  
    DefaultDenomPrecision = int64(18)  
)
```
- `Denom`: 코인의 명칭
- `CoinType`: [[BIP-44]] 표준에 따라서 이더리움이 60으로 지정되어 호환성을 위해서 60으로 사용되었다.
- `FullFundraiserPath`: [[BIP-44]] 표준에 따라 계좌와 키를 생성하기 위한 경로를 나타낸다.
	- `m`: 마스터 키를 나타낸다.
	- `purpose'`: 목적을 나타내며, BIP-44에서는 44'로 설정된다.
	- `coin_type'`: 코인의 종류를 나타내며, 이더리움과 호환성을 위해서 60으로 작성되었다.
	- `account'`: 계좌 번호를 나타낸다.
	- `change`: 외부(0) 또는 내부(1) 주소 체인을 나타낸다.
	- `address_index`: 주소의 인덱스를 나타낸다.
### config.go
- 위에서 정의한 각 타입의 config를 설정한다.
## 참고