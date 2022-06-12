## 10장 - 라이브러리

```javascript
//SPDX-License-Identifier: MIT
pragma solidity 0.8.13;
// 라이브러리 import
import "https://github.com/OpenZepplin/OpenZepplin-contracts/tree/master/contracts/math/SafeMath.sol";

contract LibrariesExample {
    // import받은 safemath를 uint변수에서 사용가능
    using SafeMath for uint;

    mapping(address => uint) public tokenBalance;

    constructor() {
        tokenBalance[msg.sender] = 1;
    }

    function sendToken(address _to, uint _amount) public returns(bool){
        tokenBalance[msg.sender] -= tokenBalance[msg.sender].sub(_amount);
        tokenBalance[_to] += tokenBalance[msg.sender].add(_amount);

        return true;
    }
}
```
