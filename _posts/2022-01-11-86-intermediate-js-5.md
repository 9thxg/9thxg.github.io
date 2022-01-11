---
title: "[JavaScript] 자바스크립트 중급5"
author: 9uTae
date: 2022-01-11 17:30:00 UTC+09:00
categories: [WebPrograming, JavaScript]
tags: [JavaScript, JS, 자바스크립트]
---

## 자바스크립트(JavaScript) 중급 5

<br>

---

<br>

### async, await

- async와 await를 사용하면 Promise를 chain형식으로 사용하는 것 보다 가독성을 좋아지게 할 수 있음

```js
// 함수 앞에 async 키워드를 붙이면 함수는 항상 Promise를 반환함
async function getName() {
     return "Mike";
}

// 함수 호출후 .then을 사용하여 실행가능
getName().then((name) => {
    console.log(name);
})

// 반환값이 Promise라면 Promise 값을 그대로 사용
async function getName() {
     return Promise.resolve("Tom");
}

// 함수 내부에서 예외가 발생하면 rejected 상태의 Promise가 발생함
async function getName() {
     throw new Error("err...");
}
// 예외가 발생했기 때문에 .then이 아닌 .catch사용
getName().catch((err) => {
    console.log(err);
})
``` 

```js
function getName(name) {
    return new Promise((res, rej) => {
        setTimeout(() => {
            res(name);
        }, 1000);
    });
}

// await 키워드는 async 함수 내부에서만 사용 가능, 일반 함수에 사용하면 오류가 발생
// await 키워드 오른쪽에는 Promise가 오고 그 Promise가 처리될 때까지 기다림
async function showName(){
    const result = await getName("Mike");
}

showName();
```

```js
const f1 = () => {
    return new Promise((res, rej) => {
        setTimeout(function() {
        console.log("1번 주문 완료");
        callback();
        }, 1000);
    });
};

const f2 = () => {
    return new Promise((res, rej) => {
        setTimeout(function() {
        console.log("2번 주문 완료");
        callback();
        }, 3000);
    });
};

const f3 = () => {
    return new Promise((res, rej) => {
        setTimeout(function() {
        console.log("3번 주문 완료");
        callback();
        }, 2000);
    });
};

// 여러 Promise를 사용한다면 result값을 다음 작업에 함께 보내줌
// 데이터 흐름이 눈에 보이기 때문에 Promise .then으로 체인하는 것보다 가독성이 좋음
async function order() {
    const result1 = await f1();
    const result2 = await f2(result1);
    const result3 = await f3(result2);
    console.log(result3);
}

order();

// 만약 작업 중간에 예외가 발생한다면 위 코드에선 오류가 출력되고 코드 진행이 멈추지만 아래와 같이 try catch문을 사용하면 에로 로그를 출력하고 코드가 진행됨
async function order() {
    try{
        const result1 = await f1();
        const result2 = await f2(result1);
        const result3 = await f3(result2);
    } catch (e) {
        console.log(e);
    }
}

// 다음과 같이 await 뒤에 Promise.all 사용가능
// 즉 async await 함수 내부에서도 비동기 함수를 병렬로 실행할 수 있음
async function order() {
    try{
        const result = await Promise.all([f1(), f2(), f3()]);
        console.log(result);
    } catch (e) {
        console.log(e);
    }
}
```

<br>

### Generator

- 함수의 실행을 중간에 멈췄다가 재개할 수 있는 기능

```js
// function 키워드 뒤에 *을 붙임(function*)
// 함수 내부에 yield 키워드를 사용하고 yield에서 함수 동작을 멈출 수 있음
function* fn() {
    yield 1;
    yield 2;
    yield 3;
    return "finish";
}

// Generator 함수를 실행하면 Generator 객체가 반환됨
const a = fn();

// Generator 객체는 .next() 메서드 사용가능
// .next() 메서드는 가장 가까운 yield문을 만날 때까지 실행하고 데이터 객체 반환
// 반환된 객체는 value와 done 프로퍼티를 가지고 value의 값은 yield의 오른쪽 값이고 만약 생략되었다면 undefined를 가짐
// done은 함수코드가 끝났는지 확인하는 프로퍼티이고 함수코드가 끝나면 true의 값을 가짐
a.next(); // {value: 1, done: false}

// {value: "finish", done: true} 이후
// 함수코드가 끝난 이후 next() 메서드를 호출하면 value는 undefined값을 가지고 done은 true의 값을 가짐
a.next(); // {value: undefined, done: true}

// Generator 함수 실행중에
// return 메서드를 호출하면 그 즉시 done은 true의 값을 가지고 value는 메서드의 인자값으로 나타난다
// 이후 next() 메서드를 실행하면 done 프로퍼티가 true이기 때문에 value값은 undefined가 된다
a.return('end'); // {value: 'end', done: true}

// error를 처리하기 위해 try catch문 사용
// throw 메서드를 실행하면 에러 로그가 출력되고 value는 undefined값을 가지고 done은 true의 값을 가짐
// 이후 next() 메서드가 실행되면 value는 undefined done은 true의 값을 가짐
function* fn() {
    try{
        yield 1;
        yield 2;
        yield 3;
        return "finish";
    } catch (e) {
        console.log(e);
    }
}
 
a.throw(new Error('err')); // 에러로그 {value: undefined, done: true}
```

- Generator는 iterable이다.
    - iterable : 반복이 가능한
    - iterable 조건
        - Symbol.iterator 메서드가 구현되어 있어야 한다.
        - Symbol.iterator 메서드가 iterator를 반환해야 한다.
    
    - iterator
        - next 메서드를 가진다.
        - next 메서드는 value와 done의 속성을 가진 객체를 반환한다.
        - 작업이 끝나면 done은 true의 값을 가진다.

- 배열도 반복이 가능하다.
    - 배열의 프로토타입을 확인하면 Symbol.iterator 메서드가 존재
    - 배열의 Symbol.iterator 메서드를 실행한 값을 이용하면 Generator와 같은 동작을 할 수 있음
    - 배열은 Symbol.iterator 메서드를 가지고 있으며 이 메서드가 iterator를 반환하기 때문에 iterable하다고 할 수 있다. 즉 배열은 반복가능한 객체이다.

- iterable한 객체는 for...of를 통해 순회할 수 있다.

- Generator 함수 값을 가지는 변수는 해당 변수가 Symbol.iterator 메서드를 실행한 값이 자기 자신이다. 즉 Generator 객체는 iterable하다.
    - gen[Symbol.iterator]() === gen // true

- 문자열 또한 프로토타입에 Symbol.iterator가 존재하기 때문에 Generator와 동일하게 사용할 수 있고 for...of를 통해 순회가능하다. 즉 문자열도 iterable하다.

```js
// next 메서드 인수 전달
// Generator는 외부로 부터 값을 입력 받을 수 있다.
function* fn(){
    const num1 = yield "첫번째 숫자를 입력해주세요";
    console.log(num1);

    const num2 = yield "두번째 숫자를 입력해주세요";
    console.log(num2);

    return num1 + num2;
}

const a = fn();

a.next(); // {value: "첫번째 숫자를 입력해주세요", done: false}
a.next(2); // {value: "두번째 숫자를 입력해주세요", done: false}
a.next(4); // {value: 6, done: true}
```

```js
// Generator는 값을 미리 만들어 놓고 있지 않음 - 메모리 관리 측면에서 효율적임
// 필요한 순간에 연산을 하여 값을 주기 때문에 - 필요한 순간까지 연산을 미룰수 있음
function* fn() {
    let index = 0;
    while(true){
        yield index++;
    }
}

const a = fn();

a.next(); // {value: 0, done: false}
a.next(); // {value: 1, done: false}
a.next(); // {value: 2, done: false}
```

```js
// yield* 를 이용하여 다른 Generator 호출
function* gen1() {
    yield "W";
    yield "o";
    yield "r";
    yield "l";
    yield "d";
}

// yield* 오른쪽에는 Generator 객체 뿐만 아니라 반복가능한 모든 객체가 올 수 있음
function* gen2(){
    yield "Hello,";
    yield* gen1();
    yield "!";
}

// "..." 구조분해할당을 사용하여 for...of와 같이 done이 true가 될 때까지 값을 펼쳐줌
console.log(...gen2()); // Hello, W o r l d !
```

- `Redux Saga`에서 Generator가 활발히 사용되고 있음

<br>

### ES 2021 자바스크립트에 추가된 새로운 기능

- String.replaceAll()
    - replace와 사용방법은 비슷하지만 replace는 만난 첫 문자만 적용되지만 replaceAll은 모든 해당 문자에 적용됨

    - replaceAll이 없었을 때

    ```js
    const str = "I'm [Mike]. This is Tom's [Car].";
    
    // 정규표현식과 g플래그(전역검색실행)를 통해 다음과 같이 변경가능했다.
    console.log(str.replace(/\[/g, "~").replace(/\]/g, "~")); // I'm ~Mike~. This is Tom's ~Car~

    // replaceAll기능을 통해 가독성이 좋아지고 쉽게 변경이 가능하다.
    console.log(str.replaceAll("[", "~").replaceAll("]", "~")); // I'm ~Mike~. This is Tom's ~Car~
    ```

- Promise.any
    - Promise.race 와 비슷하게 Promise 배열을 받아 실행한다.
    - race는 가장먼저 끝나는 결과값으로 이행/거부하지만 any는 가장 먼저 끝나는 이행된 객체를 반환한다.

- 논리 할당 연산자(Logical assignment Operaters)

    ```js
    // 다음과 같이 위식은 아래식으로 축약 가능
    // num1 += 1; 과 같은 방식으로 축약
    num1 = num1 || 0;
    num1 ||= 0;
    ```
    - "&&"와 "??"eh 가능
        - "??"는 null병합연산자(Nullish coalescing operator)로 앞부분이 null이거나 undefined일 때 뒤의 값을 가진다.

- 숫자 구분자(Numeric senerators)
    - "_"를 이용하여 숫자의 가독성을 높일 수 있음
    ```js
    let billion = 1_000_000_000 // 10억
    // 실제 사용되는 값은 1000000000으로 구분자가 없음
    ```

- WeakRef(weak reference (약한 참조))
    - 되도록이면 피하고 신중히 사용해야함
    - JS는 가비지컬렉터가 있음
        - 가비지컬렉터: 사용하지 않는 객체를 메모리에서 자동으로 해제
        - 참조가 걸려있다면 메모리 해제 X
    - 약한참조 = 가비지컬렉터 대상
    - 특정 객체를 일정시간만 캐시하는 방법으로도 이용가능
    
    ```js
    class MyCache{
        constructor(){
            this.cache = {}
        }

        add(key, obj) {
            this.cache[key] = new WeakRef(obj)
        }

        get(key) {
            let cachedRef = this.cache[key].deref()
            if(chchedRef) {
                return cachedRef
            } else {
                return false;
            }
        }
    }
    ```

<br>