## 6장 - 예외처리

 - 트랜잭션 = atomic -> 에러 발생 시 revert(일어나지 않은 일이 됨)

 1. assert
    - gas 소비 후, 특정 조건 부합하지 않으면 에러 발생
    - 내부변수, 상태 값 확인용

    ```javascript
    function receiveMoney() public payable {
        // assert : gas 소비 후, 특정 조건부합하지 않으면 에러발생(내부 변수, 상태 값 확인용)
        assert(balanceReceived[msg.sender] + uint64(msg.value) >= balanceReceived[msg.sender]);
        balanceReceived[msg.sender] += uint64(msg.value);
        }
    ```

 2. require 
    - 특정 조건 부합하지 않으면 에러 발생 후 gas 환불
    - user input값 확인용

    ```javascript
    function withdrawMoney(address payable _to, uint64 _amount) public {
        // require : 특정 조건부합하지 않으면 에러발생 후 gas환불(입력 유효성 검사용)
        require(_amount <= balanceReceived[msg.sender], "Not Enough Ether");
        balanceReceived[msg.sender] -= _amount;
        _to.transfer(_amount);
    }
    ```

 3. revert : 강제로 revert 시킬 때

 4. try / catch
    ```javascript
    contract WillThrow {

        function aFunction() pure public {
            require(false, "Error message");
        }
    }
    // try catch문
    contract ErrorHandling {
        event ErrorLogging(string reason);
        function catchError() public {
            WillThrow will = new WillThrow();
            try will.aFunction() {
                //here we could do something if it works
            }  catch Error(string memory reason) {
                emit ErrorLogging(reason);
            }
        }
    }
    ```
