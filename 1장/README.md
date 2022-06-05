## 1장 - 변수
 - 주소의 잔액은 address.balance


```javascript
    contract Variables{
    //uint 변수
    uint256 public myUint;

    function setMyUint(uint _myUint) public{
        myUint = _myUint;
    }
    
    // boolean 변수
    bool public myBool;

    function setMyBool(bool _myBool) public {
        myBool = _myBool;
    }

    uint8 public myUint8;
    
    // 0에서 빼면 256으로 돌아감
    function decrement() public {
        myUint8--;
    }

    function increment() public {
        myUint8++;
    }

    // 주소 변수
    address public myAddress;

    function setAddress(address _address) public {
        myAddress = _address;
    }
    // 주소 변수로 잔액 가져오기
    function getBalanceOfAccount() public view returns(uint) {
        return myAddress.balance;
    }

    //
    string public myString = 'hello world!';

    function setMyString(string memory _myString) public {
        myString = _myString;
    }
}
```