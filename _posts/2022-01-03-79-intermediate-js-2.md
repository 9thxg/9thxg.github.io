---
title: "[JavaScript] 자바스크립트 중급2"
author: 9uTae
date: 2022-01-03 17:30:00 UTC+09:00
categories: [WebPrograming, JavaScript]
tags: [JavaScript, JS, 자바스크립트]
---

## 자바스크립트(JavaScript) 중급 2

<br>

---

<br>

### 배열 메소드(Array method)

- arr.splice(n, m, x)
    - 특정 요소를 지움거나 지운 후 다른 요소들을 채움
    - n: 시작지점, m: 개수, x: 다른요소들
    - 만약 m이 0이라면 n앞에 x 요소들 추가
    - 삭제하는 거라면 삭제된 요소를 반환함

- arr.slice(n, m)
    - n부터 m까지 반환(n ≦ < m)
    - m을 적지 않으면 배열 끝까지 적용

- arr.concat(arr2, arr3, ...)
    - 배열들을 합쳐서 새 배열 반환
    - 여러배열도 가능
    - 배열말고 값도 가능 ([3,4],5,6)

- arr.forEach(fn)
    - 배열 반복
    ```js
    // item(요소), index(인덱스), arr(본인 배열)
    arr.forEach((item, index, arr) => {
        // 내용
    });
    ```

- arr.indexOf/arr.lastIndexOf
    - 요소를 배열에서 발견하면 반환 (가장 첫번째로 만나는 요소 반환)
    - 두 번째 인수는 시작위치를 뜻함
    - arr.lastIndexOf는 배열을 끝에서부터 탐색

- arr.includes()
    - 해당 요소를 포함하고 있는지 확인

- arr.find(fn) / arr.findIndex(fn)
    - indexOf와 비슷하지만 함수를 이용하여 보다 복잡한 연산이 가능
    - 첫번째 true 값만 반환
    - 찾지못하면 undefined 반환
    - findIndex는 해당 인덱스를 반환 없다면 -1을 반환
    - 배열 요소가 객체일 경우 유용하게 사용
    ```js
    let arr = [1,2,3,4,5];

    const result = arr.find((item) => {
        return item %2 === 0;
    })

    // 객체 배열 적용
    let userList = [
        { name: "Mike", age: 30 },
        { name: "Jane", age: 27 },
        { name: "Tom", age: 10 },
    ];

    const result = userList.findIndex((user) => {
        if(user.age < 19){
            return true;
        }
        return false;
    })
    ```
    

- arr.filter(fn)
    - find와 사용법이 동일하지만 해당하는 모든 요소를 배열로 반환

- arr.reverse()
    - 배열을 역순으로 재정렬

- arr.map(fn)
    - 함수를 받아 특정 기능을 시행하고 새로운 배열 반환
    ```js
    let userList = [
        { name: "Mike", age: 30 },
        { name: "Jane", age: 27 },
        { name: "Tom", age: 10 },
    ];

    let newUserList = userList.map((user, index) => {
        id: index + 1,
        isAdult: user.age > 19,
    });
    ```

- arr.join()
    - 배열 요소를 합쳐 문자열로 만듬
    - 인자가 없다면 구분자를 ","로 합침
    - 인자를 넣으면 인자를 구분자로 합침

- arr.split()
    - 문자열을 구분자로 나눠 배열로 변환
    - 나눌 구분자를 인자로 받음
    - 구분자를 빈문자열("")로 넣으면 한 문자씩 나눔

- Array.isArray()
    - 배열인지 아닌지 확인
    - JavaScript에서 배열은 객체형이기 때문에 typeof에서 object로 나타남
    - 따라서 배열인지 확인하기 위해서 Array.isArray() 사용

- arr.sort(fn)
    ```js
    let arr = [1,5,4,2,3];
    //let arr = ['a','c','d','e','b'];

    arr.sort(); // [1,2,3,4,5], ['a','b','c','d','e']

    // 두자리 수
    let arr = [27, 8, 5, 13];

    arr.sort(); // [13, 27, 5, 8]

    function fn(a,b){
        return a - b;{
    }

    arr.sort(fn); // [5, 8, 13, 27]

    // 다음과 같이 사용가능
    arr.sort((a,b) => {
        return a-b;
    })
    ```
    - 배열자체를 재정렬
    - 숫자, 문자열 모두 사용가능
    - 두자리 수가 있는 경우 처리시 문자열을 통해 처리하기 때문에 앞자리를 기준으로 정렬하여 이상하게 정렬됨
    - 따라서 정상 정렬하기 위해서 함수 사용
    - 주로 유용한 기능을 모아둔 라이브러리 사용(`Lodash`)

- arr.reduce(fn, x)
    ```js
    // 배열 모든수 합 계산
    let arr = [1,2,3,4,5];

    const result = arr.reduce((prev, cur) => {
        return prev + cur;
    }, 0);
    // 15

    // 객체에서 조건에 포함되는 객체 추가
    let userList = [
        { name: "Mike", age: 30 },
        { name: "Tom", age: 10 },
        { name: "Jane", age: 27 },
        { name: "Sue", age: 26 },
        { name: "Harry", age: 3 },
        { name: "Steve", age: 60 },
    ]

    let result = uesrList.reduce((prev, cur) => {
        if(cur.age > 19){
            prev.push(cur.name);
        }
        return prev;
    }, []);
    // ["Mike", "Jane", "Sue", "Harry", "Steve"]
    ```
    - 함수를 통해 누적값 계산
    - 두 번째 인자는 초기값
    - 두 번째 인자로 빈 배열 가능
    - arr.reduceRight(fn, x)로 reduce를 배열 끝에서 부터 연산 수행

<br>

### 구조 분해 할당(Destructuring assignment)

- 배열이나 객체의 속성을 분해해서 그 값을 변수에 담을 수 있게 하는 표현식

```js
let [x, y] = [1, 2];

let users = ["Mike", "Tom", "Jane"];
let [user1, user2, user3] = users; // user1 = users[0]; user2 = users[1]; user3 = users[2];

let str = "Mike-Tom-Jane";
let [user1, user2, user3] = str.split('-');

// 기본값
// c의 값이 없기 때문에 undefined 할당
let [a, b, c] = [1, 2];

// 기본값 설정
// a, b는 기본값이 있지만 값을 할당 받기 때문에 1,2를 가지고 c는 할당 받을 값이 없기 때문에 기본값 5
let [a=3, b=4, c=5] = [1, 2];

// 일부 반환값 무시
// 쉼표와 공백을 이용해서 반환값을 무시하거나 사용하는 변수가 없으면 무시됨
let [user1, ,user2] = ["Mike", "Tom", "Jane", "Tony"];

// a와 b값을 바꿀 때 유용
[a, b] = [b, a];

// 객체 구조 분해
// 객체에서는 중괄호 사용
// 값을 받을 때 순서가 바뀌어도 정상 할당(프로퍼티 이름 때문에)
let user = { name: "Mike", age: 30 };
let {name, age} = user;

// 새로운 변수 이름 할당
let {name: userName, age: userAge} = user;

// 기본값 할당
// 변수가 받을 값이 없다면 undefined 할당
let {name, age, gender='male'} = user;
```

<br>

### 나머지 매개변수(Rest parameters), 전개 구문(Spread syntax)

- 다음 기호 사용 : "..." 

- JavaScript에선 인수 전달 개수는 제한이 없음(없어도 됨)

```js
// arguments를 통한 인수 접근
// 함수로 넘어온 모든 인수 접근
// 함수내에서 이용가능한 지역변수
// Array 형태의 객체이지만 배열 내장 메소드가 없음
function showName(name){
    console.log(arguments.length); // 2
    console.log(arguments[0]); // 'Mike'
    console.log(arguments[1]); // 'Tom'
}

showName('Mike', 'Tom');

// 나머지 매개변수를 통한 인수 접근
// 정해지지 않은 개수의 인수를 배열로 나타낼수 있게함
// 아무것도 받지 못하면 undefined가 아니라 빈 배열임
// 만약 여러 종류의 인수를 받을 때 나머지 매개변수는 마지막에 위치해야함
function showName(...names){
    console.log(names);
}

showName();
showName('Mike');
showName('Mike', 'Tom');

// 전개구문
let arr1 = [1,2,3];
let arr2 = [4,5,6];

let result = [...arr1, ...arr2]; // [1,2,3,4,5,6]

// 배열 및 객체 할당시 사용 가능
let arr = [1,2,3];
let arr2 = [...arr1];

let user = {name: 'Mike', age:30};
let user2 = {...user};
```

<br>

### 클로저(Closure)

- 어휘적 환경 (Lexical Environment)

- 가장 처음 전역 Lexical 환경에 변수와 함수를 올림
    - let, const는 초기화되지 않음
    - 함수는 바로 초기화(변수에 할당한 함수 표현식은 되지않음)

- 함수가 실행될 때 하나의 내부 Lexical 환경이 생성됨
    - 이때 상위 Lexical 환경을 참조함
    - 함수가 넘겨 받은 매개변수와 지역변수가 저장됨
    - 변수를 찾을 때 내부에서 먼저 찾고 없다면 외부, 외부에도 없다면 전역 Lexical 환경까지 탐색

- 클로저(Closure)
    - 함수와 Lexical 환경의 조합
    -  함수가 생성될 때 당시의 외부 변수를 기억
    - 생성된 이후에도 계속 접근 가능

```js
// 외부 함수의 변수에 접근할 방법이 없음
// 따라서 은닉화가 된것임
function makeCounter() {
    let num = 0;

    return function () {
        return num++;
    }
}

let counter = makeCounter();

console.log(counter()); // 0
console.log(counter()); // 1
console.log(counter()); // 2
```

<br>

### setTimeout / setInterval

```js
// setTimeout은 일정시간 후 함수를 실행함
// setTimeout은 timeId를 반환
// 함수를 3초후 실행
setTimeout(fn, 3000) 

// 함수를 직접 사용해도 됨
setTimeout(function(){}, 3000);

// 만약 함수에 인수가 필요하다면 세번째 인수부터 적용가능
const tId = setTimeout(showName, 3000, 'Mike');

// 반환 받은 timeId를 통해 실행되기전 종료 가능
clearTimeout(tId);

// setInterval은 일정시간 간격만큼 함수를 실행
// setInterval 또한 timeId를 반환
const tId = setInterval(fn, 3000);

// timeId를 통해 종료 가능
clearInterval(tId);
```

- 만약 delay = 0 으로 하였다면
    - 바로 실행되지 않음
    - 현재 진행중인 스크립트가 진행되고 스케줄러가 실행되기 때문에
    - 브라우저는 기본적으로 4ms 이상의 대기시간이 있음

```js
// 1초 마다 반복되는 함수를 5초가 지났을 때 종료
// 함수에서 처리
let num = 0;

function showTime() {
    console.log(`안녕하세요. 접속하신지 ${num++}초가 지났습니다.` );
    if (num > 5){
        clearInterval(tId);
    }
}

const tId = setInterval(showTime, 1000);
```

<br>

