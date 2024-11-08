# EVM 동작 정리
## 내용
### 컨트랙트 생성
![2024-03-05 10 12 28](https://hackmd.io/_uploads/ryziEZVaa.png)
- EOA, 사용자 계정
- CA, 컨트랙트 계정
    - 상태값의 storage hash
    - evm 코드의 code hash
![2024-03-05 11 36 39](https://hackmd.io/_uploads/ry2LSWVT6.png)
- EOA가 컨트랙트를 생성한다.
- 바이트 코드가 이더리움 상태 데이터베이스(Level DB)에 저장된다.
- 트랜잭션이 컨트랙트를 호출할 때마다 EVM에 의해 실행된다.
- [컨트랙트 생성 TX 예시](https://etherscan.io/vmtrace?txhash=0xf6f2b686cf9497e61ffc9b8dfd6589b3d84106fce3ff5e758022f6f41492b520&type=parity#raw)
```jsonld
{
  "action": {
    "from": "0xb5d4b9c8294945f0c03989717e1e525d2b229463",
    "gas": "0x158f24",
    "init": "0x608060405260006006553480...",
    "value": "0x0"
  },
  "blockHash": "0x298c7947d8dfb50961787fbe57cd06c37d40236d8e2286796e6f3d69ce171afe",
  "blockNumber": 18811499,
  "result": {
    "address": "0x508e00d5cef397b02d260d035e5ee80775e4c821",
    "code": "0x608060405234801561001057600080...",
    "gasUsed": "0x158f24"
  },
  "subtraces": 0,
  "traceAddress": [],
  "transactionHash": "0xf6f2b686cf9497e61ffc9b8dfd6589b3d84106fce3ff5e758022f6f41492b520",
  "transactionPosition": 46,
  "type": "create"
}
```
### 컨트랙트 호출
![2024-03-05 14 05 27](https://hackmd.io/_uploads/S1GSOX466.png)
- tx의 input으로 함수를 실행한다.
    - input의 앞 4바이트는 function selector로 컨트랙트의 함수를 나타낸다.
- [컨트랙트 명령어 TX 예시](https://etherscan.io/vmtrace?txhash=0xc40fbd854fb70ca4f1e9b6b752227b6bfb5526298d7138f3512f2aa142260a24&type=parity#raw)
```jsonld
  {
    "action": {
      "from": "0x2f04c134d609e1adc2ad1dad13335d1a47fb43eb",
      "callType": "call",
      "gas": "0x602f",
      "input": "0x095ea7b3000000000000000000000000afa7636c0d6a79b832a11f1dddba34b1380d5e75000000000000000000000000000000000000000000000878678326eac9000000",
      "to": "0x508e00d5cef397b02d260d035e5ee80775e4c821",
      "value": "0x0"
    },
    "blockHash": "0xf0b441a3ac6dfec0dfba2cd8b764360436966b37bec19cb2e14637ca41b188f4",
    "blockNumber": 18825430,
    "result": {
      "gasUsed": "0x602f",
      "output": "0x0000000000000000000000000000000000000000000000000000000000000001"
    },
    "subtraces": 0,
    "traceAddress": [],
    "transactionHash": "0xc40fbd854fb70ca4f1e9b6b752227b6bfb5526298d7138f3512f2aa142260a24",
    "transactionPosition": 49,
    "type": "call"
  }
```
### Bootstrap of EVM in Geth
![Bootstrap of EVM in Geth](https://hackmd.io/_uploads/r1IgZxEaa.png)
- ApplyTransaction
    - 블록체인에서 트랜잭션을 처리하고 결과를 반영하여 영수증을 생성하는 과정이다.
    - https://github.com/ethereum/go-ethereum/blob/master/core/state_processor.go
    - EVM 트랜잭션 컨텍스트 생성
    - EVM 초기화
    - 트랜잭션 적용
    - 상태 업데이트
    - 영수증 생성
    - 추가 영수증 정보 설정
    - 컨트랙트 생성 주소 설정
    - 로그 및 블룸 필터설정
    - 영수증 반환
- ApplyMessage
    - 트랜잭션에 있는 메시지를 적용하고 반환한다.
    - https://github.com/ethereum/go-ethereum/blob/master/core/state_transition.go
- TransactionDb
    - 블록체인에서 트랜잭션을 처리하고 결과를 반환한다.
    - https://github.com/ethereum/go-ethereum/blob/master/core/state_transition.go
    - 사전 검사 (메시지 규칙 확인)
    - 트레이서 (디버깅 및 모니터링)
    - 가스 검사
    - 자금 검사 (잔액 확인)
    - 초기화 코드 크기 검사
    - 상태 전환 준비
    - EVM 실행
    - 가스비 환불 처리
    - 수수료 처리 (수수료 계산하고 채굴자에게 수수료 지급)
    - 실행 결과 반환
- Call
    - EVM내에서 특정주소에 연결된 컨트랙트를 실행하는 역할
    - https://github.com/ethereum/go-ethereum/blob/master/core/vm/evm.go
    - 실행 깊이 검사: 깊이 제한을 초과하는 컨트랙트 실행을 방지한다. (무한 루프 방지)
    - 잔액 검사: 호출자가 전송하는 잔액이 있는지 확인한다.
    - 상태 스냅샷 생성
    - 실행 중 오류를 대비하여 현재 상태의 스냅샷을 생성
    - 사전 컴파일된 컨트랙트 검사
    - 계정 생성 및 잔액 전송
    - 트레이서 처리
    - 컨트랙트 실행(Run)
    - 오류 처리 및 상태 롤백
    - 결과 반환
- Run
    - 호출 깊이 증가 및 복원: 함수 시작시 호출 깊이 증가, 끝나면 복원
    - 읽기 전용 모드 설정
    - 반환 데이터 초기화
    - 코드 실행 여부 결정
    - 실행 환경 설정: 메모리, 스택 및 컨텍스트 설정
    - 디버깅 모드 처리
    - 메인 루프 실행: 컨트랙트 코드를 한 명령어씩 읽어가며 실행 (인터프리터)
        - 가스비용 계산
        - 메모리 확장
        - 명령어 실행
            - input data를 읽어온다.
            - 앞 4byte는 function selector로 evm의 어떤 함수인지 확인 한다.
            - 바이트코드를 opCode로 변환한다.
            - opCode단위로 실행한다.
            - [컨트랙트 실행 TX 추적 예시](https://etherscan.io/vmtrace?txhash=0xc40fbd854fb70ca4f1e9b6b752227b6bfb5526298d7138f3512f2aa142260a24)
    - 오류 및 중단 처리
    - 결과 반환

![2024-03-05 10 17 07](https://hackmd.io/_uploads/B1oifeE6p.png)
```go
	for {
        // 점프 테이블에서 연산을 가져오고 스택의 유효성을 검사하여
        // 연산을 수행할 수 있는 충분한 스택 항목이 있는지 확인
		op = contract.GetOp(pc)
		operation := in.table[op]
		cost = operation.constantGas // 추적을 위함
		// 스택 유효성 검사
		if sLen := stack.len(); sLen < operation.minStack {
			return nil, &ErrStackUnderflow{stackLen: sLen, required: operation.minStack}
		} else if sLen > operation.maxStack {
			return nil, &ErrStackOverflow{stackLen: sLen, limit: operation.maxStack}
		}
		if !contract.UseGas(cost) {
			return nil, ErrOutOfGas
		}
		if operation.dynamicGas != nil {
			// 동적 메모리 사용량이 있는 모든 작업에는 동적 가스 비용도 있다.
			var memorySize uint64
			// 새 메모리 크기를 계산하고 
            // 연산에 맞게 메모리를 확장한다.
            // 동적 가스 부분을 평가하기 전에 메모리 검사를 수행해야 
            // 계산 오버플로를 감지할 수 있다.
			if operation.memorySize != nil {
				memSize, overflow := operation.memorySize(stack)
				if overflow {
					return nil, ErrGasUintOverflow
				}
				// 메모리는 32바이트 단위로 확장
				// 가스도 동일
				if memorySize, overflow = math.SafeMul(toWordSize(memSize), 32); overflow {
					return nil, ErrGasUintOverflow
				}
			}
			// 가스를 소비하고 사용 가능한 가스가 충분하지 않으면 오류를 반환한다.
            // 캡처 상태 지연 메서드가 적절한 비용을 얻을 수 있도록 비용이 명시적으로 설정된다.
			var dynamicCost uint64
			dynamicCost, err = operation.dynamicGas(in.evm, contract, stack, mem, memorySize)
			cost += dynamicCost // 추적을 위함
			if err != nil || !contract.UseGas(dynamicCost) {
				return nil, ErrOutOfGas
			}
			if memorySize > 0 {
				mem.Resize(memorySize)
			}
		} else if debug {
			in.evm.Config.Tracer.CaptureState(pc, op, gasCopy, cost, callContext, in.returnData, in.evm.depth, err)
			logged = true
		}
		// execute the operation
		res, err = operation.execute(&pc, in, callContext)
		if err != nil {
			break
		}
		pc++
	}
```

### 정리
- EVM
![evm.drawio](https://hackmd.io/_uploads/SkbUcNNpT.png)
- CosmWasm
![1_nyxzu7kMxpY7CIoVAio5KQ](https://hackmd.io/_uploads/r1vlDrN66.png)


## 참고
- https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf
- https://ethereum.org/en/developers/docs/evm/
- https://github.com/ethereum/wiki/wiki/%5BKorean%5D-White-Paper
- https://wonit.tistory.com/589
- https://medium.com/dsrv/cosmwasm-101-%EC%99%80%EC%94%80-1%ED%8E%B8-counter-%EC%BB%A8%ED%8A%B8%EB%9E%99%ED%8A%B8-%ED%86%BA%EC%95%84%EB%B3%B4%EA%B8%B0-e82ac42569ba
- https://hackmd.io/DZ-BcPFlSdqrFjEPFwKr0w?both