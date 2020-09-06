## Execution context

### 실행 컨텍스트
- 실행할 코드에 제공할 환경 정보를 모아놓은 객체, 컨텍스트를 구성
- 구성된 컨텍스트를 콜 스택에 쌓아 올려 전체 코드의 환경과 순서를 보장
- VariableEnvironment, LexicalEnvironment, ThisBinding이 있다.

#### VariableEnvironment
- LexicalEnvironment의 내용과 동일, 실행 컨텍스트를 생성할 때 VariableEnvironment에 정보를 저장한 후 그대로 복사하여 LexicalEnvironment를 만든다.
- LexicalEnvironment은 함수 실행 중 변경되는 사항이 즉시 반영되지만 VariableEnvironment는 초기 상태를 유지한다.

#### LexicalEnvironment
- 정적환경
- environmentRecord
    - **현재 컨텍스트**와 관련된 코드의 식별자 정보(함수, 함수의 파라미터 식별자, 변수의 식별자)들이 저장
    ```javascript
    function f (x) {      // 수집 (매개변수)
        console.log(x);
        var x;            // 수집 (변수)
        console.log(x);
        var x = 2;        // 수집 (변수)
        console.log(x);
    }  
    ```
    - 코드를 실행하기 전 식별자 정보를 수집(호이스팅)하여 자바스크립트 엔진은 이미 해당 환경에 속한 코드의 변수 식별자를 알고 있다.
    - 호이스팅
        - 끌어올리다. 자바스크립트 엔진은 식별자들을 최상단으로 끌어 올린 다음 실제 코드를 실행
        - environmentRecord는 식별자들의 선언부에 관심이 있다. 따라서 호이스팅시 변수명만 끌어 올리고 할당 과정은 원래 자리에 그대로 남겨둔다.
- outerEnvironmentReference
    - 호출된 함수가 선언될 당시의 LexicalEnvironment를 참조
    - A 함수 내부에 B 함수를 선언하고 B에 C가 선언되어 있다면 C의 outerEnvironmentReference는 B의 LexicalEnvironment를 참조 <-- 연결 리스트의 형태
    - 선언된 시점의 LexicalEnvironment를 계속 찾아 올라가면 마지막에는 전역 컨텍스트가 있을 것
    - 이와같이 가장 가까운 요소부터 차례대로 접근하여 스코프 체인이 가능

##### 스코프, 스코프 체인
- 스코프
    - 식별자에 대한 유효범위 
- 스코프 체인
    - 식별자의 유효범위를 안에서부터 바깥으로 검색해나가는 것 (outerEnvironmentReference 참조)
    - 동일한 식별자가 내부에 선언되어 있다면 스코프 체인을 통해 검색하지 않고 즉시 현재 컨택스트의 LexicalEnvironment에서 해당 식별자를 반환

##### 함수 선언문과 함수 표현식
- 함수를 정의
```javascript
// 함수 선언문, 함수명 a가 변수명
function a () {
}
// 함수 표현식, 변수명 b가 함수명
var b = function () {
}
// 기명 함수 표현식, 함수명은 d, 변수명은 c
// 외부에서는 함수명이 아닌 변수명으로 호출
var c = function d () { 
}
```
- 함수 선언문과 함수 표현식의 호이스팅
```javascript
// 원본 코드
console.log(sum(1, 2));
console.log(multiply(3, 4));    // TypeError: multiply is not a function 발생

function sum (a, b) { return a+b; }

var multiply = function (a, b) { return a*b; }

// 호이스팅된 상태
// 함수 선언문 : 함수 전체가 호이스팅 되었다.
// 함수 선언문은 전역 공간에 선언된 함수들이 모두 가장 위로 끌어올려지고 
// 동일한 변수명에 다시 함수를 할당 할 경우 나중에 할당한 값이 먼저 할당한 값을 덮어 씌운다.
// 간혹 이에따른 버그 발생 가능성도 있으니 참고하자.
var sum = function sum (a, b) { return a+b; }

// 함수 표현식 : 변수 선언부만 호이스팅 되었다.   
var multiply;                                   

console.log(sum(1, 2));
console.log(multiply(3, 4));    // TypeError: multiply is not a function 발생

multiply = function (a, b) { return a*b; }
```

---
### Reference
정재남. _코어 자바스크립트_. 위키북스  
