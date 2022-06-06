## 2장 - 송금/수신
 
### payable함수는 두가지
 - payable 붙은 것에서만 송금 / 수신 가능
 
 1. 함수형
 2. 주소형 : send(예외처리불가), transfer 함수 내장

```javascript
   pragma solidity ^0.8.14;

contract SendMoney {

    uint public balanceReceived;
    uint public lockedUntil;

    // payable이 붙은것에만 코인 전송 가능
    // 1. 함수 payable
    function receiveMoney() public payable{
        balanceReceived += msg.value;
        //lockedUntil 값을 노출
        lockedUntil = block.timestamp + 1 minutes;
    }

    function getBalance() public view returns(uint){
        return address(this).balance;
    }

    function withdrawMoney() public {
    // 2. 주소 payable (send, transfer 함수 내장)
        if(lockedUntil < block.timestamp){
            address payable to = payable(msg.sender);
            to.transfer(this.getBalance());
        }
    }

    // 주소를 받아서 해당주소에 전송
    function withdrawMoneyTo(address payable _to) public{
        if(lockedUntil < block.timestamp){
            _to.transfer(this.getBalance());
        }
    }
}
```