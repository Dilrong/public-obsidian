## 요약
- `InstantiateContract`에서 `"marketing": { "marketing": "xpla14hsul4m2dxm3nfgav4sas2ts5lg5p22ek4lust" }`을 추가한다.
## 내용
- cw20에서 `update_marketing`, `upload_logo`에 해당하는 execute 명령어를 보내면 `failed to execute message; message index: 0: Unauthorized: execute wasm contract failed [CosmWasm/wasmd@v0.45.0/x/wasm/keeper/keeper.go:395] With gas wanted: '100000000' and gas used: '116906'` 오류 발생하면서 execute 명령어를 보낼 수 없었음
- xpla 디스코드의 dev-discussion에 해당하는 부분 다음과 같이 문의함
```zsh
hello. I'm trying to do `update_marketing` or `upload_logo` with execute on a cw20 I made, but I'm getting an Unauthorizederror message.

`failed to execute message; message index: 0: Unauthorized: execute wasm contract failed [CosmWasm/wasmd@v0.45.0/x/wasm/keeper/keeper.go:395] With gas wanted: '100000000' and gas used: '116906'`

I set both Minter and Admin to my wallet address, and I don't know why I'm getting the above error. contract address: `xpla1yrs56zmdd43auvdvu5p7amprffk52yx3sn7rxpchgxta4p9j2vysymcmz5`

`{   "name": "minter_test",   "symbol": "MINTER",   "decimals": 6,   "initial_balances": [     {       "address": "xpla14hsul4m2dxm3nfgav4sas2ts5lg5p22ek4lust",       "amount": "10000"     }   ],   "mint": {     "minter": "xpla14hsul4m2dxm3nfgav4sas2ts5lg5p22ek4lust",     "gap": "10000"   } }`
```
- you should've add up `marketing` field when initializing, like `"marketing": { "marketing": "xpla14hsul4m2dxm3nfgav4sas2ts5lg5p22ek4lust" }`
- 위와 같이 답변으로 `InstantiateContract`에서 `"marketing": { "marketing": "xpla14hsul4m2dxm3nfgav4sas2ts5lg5p22ek4lust" }`을 추가하면 된다고 답변을 받음
## 참고
- 