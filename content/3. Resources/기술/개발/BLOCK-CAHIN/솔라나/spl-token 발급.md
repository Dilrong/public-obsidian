## 요약
- spl token 발급 방법
## 내용
### SPL CLI
- solana 설정
    - rust install
    - `sh -c "$(curl -sSfL https://release.solana.com/v1.18.4/install)"`
    - `solana --version`
    - 솔라나 지갑 추가 `solana-keygen new --outfile my_wallet.json`
- spl 설정
    - `cargo install spl-token-cli`
    - `solana config get`
    - 데브넷 설정 `solana config set --url https://api.devnet.solana.com` 
    - 키페어 설정 `solana config set --keypair ${HOME}/new-keypair.json`
- spl 배포
    - `solana airdrop 1`
    - `spl-token create-token`
        - Address: Ht7XwHTWxAWSMUKi8gaPKkhEARowSxSeBadGaZMTSy4k
    - 발행량 확인 `spl-token supply Ht7XwHTWxAWSMUKi8gaPKkhEARowSxSeBadGaZMTSy4k`
    - 토큰 계정 생성 `spl-token create-account Ht7XwHTWxAWSMUKi8gaPKkhEARowSxSeBadGaZMTSy4k`
        - Account: EPMbMxYzU4qquhmLwWcAdRmdq9Jbr51iFzi1fWEDRyMY
    - 토큰 수량 확인 `spl-token balance Ht7XwHTWxAWSMUKi8gaPKkhEARowSxSeBadGaZMTSy4k`
    - 100개 민팅 `spl-token mint Ht7XwHTWxAWSMUKi8gaPKkhEARowSxSeBadGaZMTSy4k 100`
    - 현재 계정이 소유한 토큰 보기 `spl-token accounts`
- spl 옵션
    - 프로그램 정보 확인 `solana program show Ht7XwHTWxAWSMUKi8gaPKkhEARowSxSeBadGaZMTSy4k`
    - 번 `spl-token burn EPMbMxYzU4qquhmLwWcAdRmdq9Jbr51iFzi1fWEDRyMY 100`
        - EPMbMxYzU4qquhmLwWcAdRmdq9Jbr51iFzi1fWEDRyMY는 민팅에서 나온 Recipient이다.
        - Recipient는 계정에 저장된 토큰 해시값이다.
    - 계정 닫기 `spl-token close H6TFTSmzYnjqoWFT8omducQMpYfscWxPHcXssbG7xvcW`
        - 모든 토큰이 번 되어야지 가능하다.
    - 계정 권한 변경 `spl-token authorize token_address owner user_address`
## 참고
- https://docs.solanalabs.com/cli/install
- https://spl.solana.com/token
- 익스플로어
    - https://solana.fm/
    - https://explorer.solana.com/
    - https://solscan.io/