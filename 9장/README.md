## 9장 - event

 1. event 사용이유
    - 실제 트랜잭션에서는 return값을 넣어도 트랜잭션 외부에 return 불가

    1. 컨트랙트 외부에 return값 주기 위해(output logs) - DAPP에서 사용
    2. 특정 함수 트리거
    3. 임시 데이터 저장

2. 데이터저장 비용 해결방법
    - 오프체인에 저장
    - IPFS에 저장
    - 이벤트 로그에 저장

3. event 특징
    - 상속불가
    - 인수에 인덱스 넣어서 검색 가능
    - 비용이 쌈

    ```javascript
    //SPDX-License-Identifier: MIT
    pragma solidity 0.8.13;

    contract EventExample {

        mapping(address => uint) public tokenBalance;

        // 실제 트랜잭션에서는 return값을 넣어도 ouput에 반영 X
        // ouput을 주기 위해서는 event가 필요
        // 1. 컨트랙트 외부에 return값 주기 위해 - dapp에게 정보 전달
        // 2. 함수 트리거
        // 3. 임시 데이터 스토리지
        // 블록체인에서 데이터 저장은 비쌈
        // - 오프체인에 저장 / IPFS에 저장 / 이벤트 로그에 저장
        // 상속불가
        // 인수에 인덱스를 넣어서 나중에 검색가능
        event TokensSent(address _from, address _to, uint _amount);

        constructor() {
            tokenBalance[msg.sender] = 100;
        }

        function sendToken(address _to, uint _amount) public returns(bool) {
            require(tokenBalance[msg.sender] >= _amount, "Not enough tokens");
            assert(tokenBalance[_to] + _amount >= tokenBalance[_to]);
            assert(tokenBalance[msg.sender] - _amount <= tokenBalance[msg.sender]);
            tokenBalance[msg.sender] -= _amount;
            tokenBalance[_to] += _amount;
            // emit으로 이벤트 출력 > 트랜잭션 output log 에 찍힘
            emit TokensSent(msg.sender, _to, _amount);

            return true;
        }
    }
    ```
