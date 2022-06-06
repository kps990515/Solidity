## 4장 - 매핑, 배열 enum
 
 1. 매핑 
    - key,value 구조
    - 길이 가변형

    ```javascript
    mapping(uint => bool) public myMapping;
    mapping(address => bool) public myAddressMapping;

    function setValue(uint _index) public {
        myMapping[_index] = true;
    }

    function setMyAddressToTrue() public {
        myAddressMapping[msg.sender] = true;
    }
    ```

 2. 배열
    - 고정 / 가변 사이즈 둘다 가능
    - length, push 함수 내장
    - 가스비가 비쌈

    ```javascript
    address[] public addressArray; 
    address[5] public addressArray;
    ```

 3. enum : 가장 작은 크기로 할당됨(255개 -> uint8)
```javascript
   enum ActionChoices {Left, Right}
    ActionChoices choices;
    ActionChoices constant defaultChoice = ActionChoices.Left;
```
