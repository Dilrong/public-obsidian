## 요약
- 블록체인과 상호운용이 가능한 모듈 (SDK)
## 내용
- 블록체인과 상호운용이 가능한 모듈 (SDK)
- [[CometBFT]]은 네트워크, 합의 레이어
- CosmosSDK는 애플리케이션 레이어
- 라고 생각해도 좋다.
```
                ^  +-------------------------------+  ^
                |  |                               |  |
                |  |  State-machine = Application  |  |
                |  |                               |  |   Built with Cosmos SDK
                |  |            ^      +           |  |
                |  +----------- | ABCI | ----------+  v
                |  |            +      v           |  ^
                |  |                               |  |
Blockchain Node |  |           Consensus           |  |
                |  |                               |  |
                |  +-------------------------------+  |   CometBFT
                |  |                               |  |
                |  |           Networking          |  |
                |  |                               |  |
                v  +-------------------------------+  v
```
## 참고
- https://docs.cosmos.network/
- https://consensusmymem.tistory.com/57