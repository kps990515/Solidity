## 3장 - 스마트컨트랙트 시작/중단/파괴
 
 1. constructor함수 : 컨트랙트 배포 맨 처음에 한번만 실행되는 생성자
    ```javascript
    constructor() public {
        owner = msg.sender;
    }
    ```

 2. require함수 : if문과 동일, 조건체크하고 틀리다면 transaction rollback
    ```javascript
    // 여기서 this는 msg.sender가 아닌 스마트컨트랙트 주소
    function withdrawAllMoney(address payable _to) public {
        //require문 만나면 조건 체크하고 틀리다면 트랜잭션 롤백
        require(msg.sender == owner, "You are not the owner");
        require(paused == false, "Contract is paused");
        _to.transfer(address(this).balance);
    }
    ```

 3. selfdestruct 함수 : 스마트컨트랙트 destroy하는 함수, 인자로 스마트 컨트랙트 잔액 반환받을 주소 필요
```javascript
    function destroySmartContract(address payable _to) public {
        require(msg.sender == owner, "You are not the owner");
        // selfdestruct : 스마트컨트랙트 destroy하는 함수, 인자로 스마트 컨트랙트 잔액 반환받을 주소 필요
        selfdestruct(_to);
    }
```

```javascript
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.1;

contract StartStopUpdateExample {

    address owner;
    bool public paused;

    // 컨트랙트 배포 맨 처음에 한번만 실행되는 생성자
    constructor() public {
        owner = msg.sender;
    }

    function sendMoney() public payable {

    }

    function setPaused(bool _paused) public {
        require(msg.sender == owner, "You are not the owner");
        paused = _paused;
    }

    // 여기서 this는 msg.sender가 아닌 스마트컨트랙트 주소
    function withdrawAllMoney(address payable _to) public {
        //require문 만나면 조건 체크하고 틀리다면 트랜잭션 롤백
        require(msg.sender == owner, "You are not the owner");
        require(paused == false, "Contract is paused");
        _to.transfer(address(this).balance);
    }

    function destroySmartContract(address payable _to) public {
        require(msg.sender == owner, "You are not the owner");
        // selfdestruct : 스마트컨트랙트 destroy하는 함수, 인자로 스마트 컨트랙트 잔액 반환받을 주소 필요
        selfdestruct(_to);
    }
}
```