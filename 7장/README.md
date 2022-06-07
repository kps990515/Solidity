## 7장 - fallback 함수, view/pure 함수
 - Writing Transaction = transaction
 - Reading Transaction = call(로컬 블록체인을 읽는 것이므로 가스비 무료)

 1. fallback 함수
    - external 만 사용(외부에서 호출할때 작동하기 때문에)
    - fallback 함수 내부에서 처리 가능한 트랜잭션은 최대 2300gas
    - require(msg.data.length == 0)을 넣어서 외부에서 없는 함수 호출하거나 데이터 없는 트랜잭션 호출 방지 가능

    1. receive 함수 : 이더 수신 시 트랜잭션 데이터가 없는 경우 작동
    ```javascript
    receive() external payable {
        receiveMoney();
    }
    ```

    2. fallback 함수 : 이더 송신 시 없는 함수 호출할때
        - payable은 옵션(이 경우에는 없는 함수 호출할 때 작동)
    ```javascript
    fallback() external payable {

    }
    ```

 2. view 함수 : 클래스(저장소) 변수 접근가능
     ```javascript
    function getOwner() public view returns(address) {
        return owner;
    }
    ```

 3. pure 함수 : 클래스(저장소) 변수 접근 X 인자만 활용
    ```javascript
    function convertWeiToEth(uint _amount) public pure returns(uint) {
        return _amount / 1 ether;
    }
    ```
