---
title: "[JavaScript] 자바스크립트 중급1"
author: 9uTae
date: 2021-12-30 17:30:00 UTC+09:00
categories: [WebPrograming, JavaScript]
tags: [JavaScript, JS, 자바스크립트]
---

## 자바스크립트(JavaScript) 중급 1

<br>

---

<br>

### 변수

- var는 한번 선언된 이후 다시 선언 가능
```js
var num = 1;
console.log(num);

var num = 10;
console.log(num);
```

- var는 선언하기 전에 사용가능
```js
console.log(num);

var num = 10;
```

- 호이스팅(hoisting)
    - 스코프 내부 어디서든 변수 선언은 최상위에 선언된 거 처럼 행동
    - 선언은 호이스팅 되지만 할당은 호이스팅 되지않는다.

- TDZ(Temporal Dead Zone)
    - 변수의 선언과 초기화 사이의 변수에 접근할 수 없는 지점
    - 초기화 되지않은 변수가 있는 곳
    - let과 const는 TDZ 영향을 받는다.

- 변수 생성단계
    - 1. 선언단계 2. 초기화단계 3. 할당 단계
    - var : 1. 선언 및 초기화 2. 할당
    - let : 1. 선언 2. 초기화 3. 할당
    - const : 1. 선언, 초기화, 할당
    - 따라서 let은 선언과 초기화 단계가 따로 되어있기 때문에 var처럼 TDZ의 영향을 안받을 수 없음

- 스코프
    - var : 함수스코프(function-scoped)
        - 함수내에서만 유효
    - let, const : 블록스코프(block-scoped)
        - 함수, if문, for문, while문, try/catch문 등과 같이 블록으로 지정된 곳에서만 유효

<br>

### 생성자 함수
- 비슷한 객체를 여러개 만들 때 사용

- 첫 문자를 대문자로 하는것이 관례

```js
function User(name, age){
    // this = {};

    this.name = name;
    this.age = age;
    this.sayName = function() {
        console.log(this.name);
    }
    // return this;
}
```
- 사용시 `new`를 사용(new 사용시 위코드에서 주석처리된 부분이 자동으로 처리된다.)

```js
let user1 = new User('Mike', 30);
```

- `new`를 붙이지 않으면 함수 실행이기 때문에 undefined를 반환함

<br>

### 객체 메소드와 계산된 프로퍼티
- 계산된 프로퍼티(Computed Property)
```js
// Computed Property
let a = 'age';
const user = {
    name : 'Mike',
    [a] : 30,
    [1+4] : 5,
    ["안녕" + "하세요"] : "Hello"
}

// 객체를 만드는 함수에서 활용
// 객체의 키값을 모를 때 유용힘
function makeObj(key, val){
    return {
        [key] : val
    }
}
```

### 객체 메소드
- Object.assign()
    - 객체 복제
    ```js
    // 다음과 같이 대입하게되면 주소값이 대입되어 결국 같은 객체를 가리키게 됨
    const cloneUser = user;

    // Object.assign 사용
    const cloneUser = Object.assign({}, user);
    ```
    - 첫번째 인자는 초기값, 두번째인자부터 복제할 값이 있는 객체들

    ```js
    // 초기값에 다른값이 있다면 그 값에 더해서 나머지가 복제됨
    const cloneUser = Object.assign({gender : 'male'}, user);

    // 여러 객체를 한 객체에 복제 가능
    Object.assign(user, info1, info2);
    ```
    - 복제할 키값이 기존객체에 존재할 경우 값을 덮어씌움

- Object.keys()
    - 키 배열 반환

- Object.values()
    - 값 배열 반환

- Object.entries()
    - 키와 값에 대한 배열 반환(2차원 배열)

- Object.fromEntries()
    - 키와 값 배열을 객체로 변환
    ```js
    const arr = [
        ["name", "Mike"],
        ["age", 30],
        ["gender", "male"]
    ]

    Object.fromEntries(arr);
    ```
<br>

### 심볼

- 유일한 식별자
- 유일성 보장(전체 코드중에 하나 존재)

```js
// 같은 값처럼 보이지만 다름 (a===b, a==b / false, false)
const a = Symbol(); // new 를 사용하지 않음!
const b = Symbol();

// 생성시 심볼에 대한 설명 추가가능
const id = Symbol("description");

// Object.keys, values, entries, for...in 등 조회할 때 Symbol 프로퍼티는 건너 뜀
// [id]에 대한 출력이 나타나지 않음
const user = {
    name : "Mike",
    age: 30,
    [id] : "myid"
}
```

- 전역 심볼 Symbol.for()
    - 하나의 심볼만 보장받을 수 있음
        - 없으면 만들고, 있으면 가져오기 때문에
    - 하나를 생성한 뒤 키를 통해 같은 Symbol 공유
    ```js
    // id1===id2 true
    const id1 = Symbol.for('id');
    const id2 = Symbol.for('id');
    ```
    - 생성할 때의 값 확인 가능
        - Symbol.keyFor(id1); // "id"


- Symbol은 a.description 을 통해 생성할 때 설명값 확인 가능

- 숨겨진 Symbol key 확인 방법
    ```js
    Object.getOwnPropertySymbols(user); // [Symbol(id)]
    Reflect.ownKeys(user); // ["name", "age", Symbol(id)]
    ```

- Symbol을 사용할 때의 장점
    - 기존의 객체에 같은 이름의 프로퍼티, 메소드가 있는지 고민할 필요가 없음
    - 기존의 객체에 프로퍼티, 메소드를 덮어 쓸일이 없어짐

<br>

### 숫자, 수학
- 10진수를 2진수 또는 16진수로 바꿀 때
```js
let num = 10;
num.toString(); // "10"
num.toString(2); // "1010"

let num2 = 255;
num.toString(16); // "ff"
```

- Math
    - Math.PI
        - 원주율
    - Math.ceil()
        - 올림
        - Math.ceil(6.2); // 7 
    - Math.floor()
        - 내림
        - Math.floor(6.2); // 6
    - Math.round()
        - 반올림
        - Math.round(6.2); // 6
    - Math.random()
        - 0~1 사이의 무작위 숫자 생성
    - Math.Max() / Math.Min()
        - 괄호안의 숫자 중 최대값, 최소값을 구함
    - Math.abs()
        - 절대값
    - Math.pow(n, m)
        - 제곱
        - n**m
    - Math.sqrt()
        - 제곱근

- 소수점 반올림 하는 법
    ```js
    // 소수점 둘째자리 까지 표현
    let userRate = 30.1234;

    Math.round(userRate*100) / 100 // 30.12

    // toFixed 메소드 사용가능
    // 단 결과값이 문자열이기 때문에 형변환 필요
    userRate.toFixed(2);

    // 소수부 개수보다 큰수를 넣으면 남은 수 만큼 0을 채움
    userRate.toFixed(7); // 30.1234000
    ```

- isNaN()
    - NaN은 본인끼리 조차 다르다고 표현
    - isNaN()만이 NaN인지 확인 가능

- parseInt()
    ```js
    // 숫자로 시작하는 문자열은 읽을 수 있는 숫자까지 읽음
    // 문자로 시작한다면 아무것도 읽지 못함
    let marign = "10px";
    parseInt(margin); // 10

    // 만약 문자열이 16진수일 경우 두번째 인자를 16진수로 설정하여 읽기 가능
    let redColor = 'f3';
    parseInt(redColor, 16); // 243
    ```

- parseFloat()
    - parseInt()와 동일하게 동작하지만 소수점까지 읽어옴
<br>

### 문자열
- ""이 포함된 내용이 많다면 '' 사용
- 영어와 같이 '' 사용이 많다면 "" 사용
- 문자열에 변수를 입력하여 사용하거나 여러줄을 쉽게 표현하고싶다면 ``(backtick) 사용

- str.length
    - 문자열 길이값

- 배열과 같이 특정위치 접근 가능
    - str[2]

- 문자열이 영어인 경우 toUpperCase() / toLowerCase() 를 사용하여 문자열 모두 대문자, 소문자로 변경가능
    - str.toUpperCase() / str.toLowerCase()

- str.indexof(text)
    - 해당 text의 인덱스를 반환
    - 없을시 -1 반환
    - 있다면 첫번째 위치 반환

- str.includes(text)
    - 해당 text가 포함되어있는지 확인
    - str.indexof(text)로 구현가능
    ```js
    if(str.indexof(text) > -1)
    ```

- str.slice(n, m)
    - 문자열에서 n에서부터 m까지 문자열 반환
    - m값이 있다면 m까지 반환
    - m값이 없다면 끝까지 반환
    - m값이 음수라면 끝에서부터 셈

- str.substring(n, m)
    - n과 m사이의 문자열 반환
    - n과 m이 바뀌어도 동일하게 동작
    - 음수는 0으로 취급

- str.substr(n, m)
    - n에서 시작하여 m개를 가져옴
    - n이 음수 일때 문자열 끝에서부터 인덱스는 -1로 취급하여 m개 개수만큼 가져옴

- str.trim()
    - 문자열 앞뒤 공백 제거

- str.repeat(n)
    - 문자열을 n번 반복함

- 문자는 비교할 수 있음
    - 문자의 숫자코드를 알고싶다면
        - "a".codePointAt(0); // 97
    - 숫자코드에 대한 문자를 알고싶다면
        - String.fromCodePoint(97); // "a"
        
<br>

