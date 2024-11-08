## 요약

-   trustless.computer는 Taproot를 이용하여 블록체인에 데이터를 삽입하여 동작하는 가상머신을 구현합니다.
-   핵심기술이라고 볼 수 있는 BVM은 TxReader > Ethereum-compatible VM > Local mempool > TxWriter로 구성되어 있습니다.
-   이더리움의 솔리디티 언어로 BRC, 비트코인 기반 디앱을 구현할 수 있습니다.

## 개요

> _비트코인을 단순 통화이상으로 사용할 수 있도록 일반화하는 것을 목표로 하는 레이어1 프로토콜로 비트코인에서 Dapp을 만들 수 있도록 도와줍니다._

비트코인은 가장 탈중앙화되고 안전한 블록체인으로 널리 알려져 있습니다. 그러나 프로토콜의 데이터 형식은 제한되어 있어서 다른 응용 프로그램을 확장하기 어렵습니다.

![https://brc-20.io/](https://images.mirror-media.xyz/publication-images/F4u2TIuKdNcWBIyWvAKBI.png?height=733&width=1066)

하지만 최근에는 오디널스, BRC-20 등 비트코인을 이용한 다양한 애플리케이션을 볼 수 있습니다.

트러스트.컴퓨터(trustless.computer)는 탭루트(Taproot)를 이용하여 비트코인 블록체인에 모든 데이터를 삽입하여 비트코인 위에서 동작하는 가상머신을 구현합니다.

댑루트에 대해서 간단하게 설명하지만 소프트 포크(soft fork)입니다.

소프트 포크를 하면 기존 블록체인의 구조는 변경되지 않고 부분적인 기능개선만을 이루기 때문에 블록체인 노드는 간단한 시스템 업그레이드만으로 새로운 시스템으로 이전이 가능합니다.

## 아키텍처

![](https://images.mirror-media.xyz/publication-images/zG8CPX0k7ycDawTrJwZFu.png?height=1600&width=1138)

트러스트.컴퓨터는 BVM이 핵심이며 이를 블록체인 데이터 계층으로 활용하며 EVM(Ethereum-VM)과 유사한 상태 머신입니다. 4가지 구성 요소 mempool, VM, TxWriter, TxReader를 활용합니다.

## VM

BVM은 이더리움 호환 가상 머신으로 더 큰 트랜잭션, 더 높은 블록 가스 한도를 지원하도록 구성되어 더 많은 종류의 애플리케이션을 지원합니다.

## Local Mempool

각 노드는 사용자가 로컬 멤풀(Local mempool)의 노드로 보내는 트랜잭션 모음을 유지 관리합니다.

비트코인 네트워크에 기록되기 전에 거래의 유효성을 확인하는데 도움이 됩니다.

이 멤풀은 다른 노드와 공유되지 않기 때문에 로컬입니다. 트랜잭션은 비트코인 트랜잭션(TxWriter)에 기록되고 비트코인 네트워크에 브로드캐스팅될 때만 공개적으로 노출됩니다.

## TxReader

모든 새로운 비트코인 블록에서 BVM트랜잭션을 필터링하고 BVM 상태가 네트워크의 모든 BVM 노드에서 일관되도록 보장합니다.

각 비트코인 블록에 대해 BVM은 먼저 BVM 트랜잭션을 필터링하고 가스 요금별로 정렬한 다음 VM에 BVM 블록으로 가져옵니다. 비트코인에는 불변의 결정적 순서가 있으므로 코드베이스를 실행하는 모든 BVM 노드는 동일한 상태를 갖습니다.

비트코인 재구성(reorg)는 두 명 이상의 채굴자가 동시에 다른 블록을 블록체인에 추가할 때 발생하는 프로세스입니다. 이로 인해 네트워크의 다른 부분의 다른 버전의 블록체인이 있는 체인에 분기가 발생할 수 있습니다. TxReader는 BVM 상태를 포크된 분기 지점으로 되돌린 다음 유효한 블록을 되돌린 상태로 다시 가져와 비트코인 재구성을 처리하도록 설계되었습니다.

## TxWriter

TxWriter를 사용하면 BVM 트랜잭션을 비트코인 트랜잭션에 포함할 수 있습니다. 이를 달성하기 위해 오디널스가 비트코인 네트워크에 데이터를 입력하는데 사용하는 유사한 기술을 사용합니다.

기본적으로 BVM 트랜잭션 데이터는 감시(witness) 데이터 필드를 통해 비트코인 트랜잭션에 포함됩니다. 이 데이터는 인증 프로세스에 영향을 주지 않고 트랜잭션 로직에 영향을 미치지 않는 방식으로 수행됩니다.

감시 데이터 필드는 이전 출력에 대한 잠금 해제 스크립트로만 노출됩니다. 비트코인 트랜잭션에 데이터를 넣으려면 2단계 트랜잭션 프로세스를 수행해야합니다.

1.  커밋 트랜잭션으로, 지출에 대한 감시/스크립트 해시를 나타내는 미사용 트랜잭션(UTXO)을 생성하는 것입니다.
    
2.  공개 트랜잭션이 BVM 트랜잭션이 포함된 감시 데이터/스크립트를 노출합니다.
    

## BRC-20 만들기

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract Bitswap is ERC20 {
    constructor(uint256 initialSupply) ERC20("Bitswap", "BIT") {
        _mint(msg.sender, initialSupply);
    }
}
```

이더리움에서 ERC20 코드와 같으며 네트워크만 바꿔서 배포해주시면 됩니다.

hardhat.config.ts 기준 네트워크 구성은 다음과 같습니다.

```
  networks: {
    mynw: {
      url: "http://localhost:10002",
      accounts: {
        mnemonic: "<your mnemonic with funds>"
      },
      timeout: 100_000,
    },
    blockscoutVerify: {
    	blockscoutURL: "http://localhost:4000", // your explorer URL
    	...
    }
  }
```

## 결론

오디널스 이후로 비트코인을 활용한 기술발전이 많이 일어나고 있으며 하이프도 많이 붙은 것을 볼 수 있습니다.

오디널스나 탭루트 등 비트코인을 활용한 기술이 발전하고 이를 이용한 디앱도 조금씩 나오는 만큼 관심을 가지고 지켜보는 것을 좋다고 생각합니다.

## 참고

http://wiki.hash.kr/index.php/%EC%86%8C%ED%94%84%ED%8A%B8%ED%8F%AC%ED%81%AC

https://trustless.computer/

https://trustless.computer/developer

https://docs.trustless.computer/trustless-computer/overview