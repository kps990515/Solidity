## 8장 - modifier, 상속, import

 1. modifier 함수
    - 인수를 받을 수 있는 함수 제어자
    - 함수가 modifier 만나면 modifier함수에 있는 pre-condition체크 후 함수 실행

    ```javascript
    contract Owned {
        address owner;

        constructor() {
            owner = msg.sender;
        }

        // modifier: 인수를 받는 함수제어자
        // 함수가 modifier만나면 modifier 함수 내부 로직을 함수로 가져온다
        modifier onlyOwner() {
            require(msg.sender == owner, "You are not allowed");
            _;
        }
    }
    ```

2. 상속
    - 다중상속가능(A is X,Y,Z)
    - XYZ 동일 함수 있는 경우 Z가 최우선
    - super로 부모 접근 가능
    - 부모 컨트랙트도 자식과 합쳐져 하나의 컨트랙트로 배포
    
    ```javascript
    // 상속
    contract InheritanceModifierExample is Owned{

        mapping(address => uint) public tokenBalance;

        uint tokenPrice = 1 ether;

        constructor() {
            tokenBalance[owner] = 100;
        }
        //moidifier
        function createNewToken() public onlyOwner{
            tokenBalance[owner]++;
        }
        //moidifier
        function burnToken() public onlyOwner{
            tokenBalance[owner]--;
        }

        function purchaseToken() public payable {
            require((tokenBalance[owner] * tokenPrice) / msg.value > 0, "not enough tokens");
            tokenBalance[owner] -= msg.value / tokenPrice;
            tokenBalance[msg.sender] += msg.value / tokenPrice;
        }

        function sendToken(address _to, uint _amount) public {
            require(tokenBalance[msg.sender] >= _amount, "Not enough tokens");
            assert(tokenBalance[_to] + _amount >= tokenBalance[_to]);
            assert(tokenBalance[msg.sender] - _amount <= tokenBalance[msg.sender]);
            tokenBalance[msg.sender] -= _amount;
            tokenBalance[_to] += _amount;
        }
    }
    ```

 3. import 
     ```javascript
    import "./Owned.sol";
    import * as symbolName from "filename";
    import {symbol1 as alias, symbol2} from " filename";
    ```
