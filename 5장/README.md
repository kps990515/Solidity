## 5장 - 구조체와 매핑
 
 - contract는 최대한 적게 만들자(가스비 비쌈)

 #### 구조체
    - 데이터 형식 자유롭게 구현
    - 구조체 안의 구조체는 불가

    ```javascript
    //SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;
// 컨트랙트는 최대한 적게 만들자(가스비)
contract MappingsStructExample {
    // 구조체
    // 구조체 안의 구조체는 불가
    struct Balance {
        uint totalBalance;
        uint numPayments; //payments 번호
        mapping(uint => Payment) payments; //payments mapping
    }

    struct Payment {
        uint amount;
        uint timestamp;
    }

    mapping(address => Balance) public balanceReceived;

    function getBalance() public view returns(uint) {
        return address(this).balance;
    }

    function sendMoney() public payable {
        // msg.value는 트랜잭션에서 이동한 eth를 의미
        balanceReceived[msg.sender].totalBalance += msg.value;
        // block.timestamp : 현재시간
        Payment memory payment = Payment(msg.value, block.timestamp);
        // Balance 접근 > Balance의 payments번호를 통해 payments매핑 접근 > 해당 인덱스에 payment 정보 입력 
        balanceReceived[msg.sender].payments[balanceReceived[msg.sender].numPayments] = payment;
        balanceReceived[msg.sender].numPayments++;
    }

    function withdrawMoney(address payable _to, uint _amount) public {
        require(balanceReceived[msg.sender].totalBalance >= _amount, "not enough funds");
        balanceReceived[msg.sender].totalBalance -= _amount;
        _to.transfer(_amount);
    }
    
    function withdrawAllMoney(address payable _to) public {
        uint balanceToSend = balanceReceived[msg.sender].totalBalance;
        // 외부주소와 상호작용할때는 해당 로직&데이터 업데이트 먼저, 그리고 상호작용
        balanceReceived[msg.sender].totalBalance = 0;
        _to.transfer(balanceToSend);
    }
}
    ```
