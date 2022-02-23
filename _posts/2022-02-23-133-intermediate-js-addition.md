---
title: "[JavaScript] 자바스크립트 응용 추가내용"
author: 9uTae
date: 2022-02-23 17:30:00 UTC+09:00
categories: [WebPrograming, JavaScript]
tags: [JavaScript, JS, 자바스크립트]
---

## 자바스크립트(JavaScript) 응용 추가내용

<br>

---

<br>

### 콜백함수

- 함수 파라미터(매개변수)로 함수를 이용하는 것

- 여러가지 유연한 동작가능한 함수를 만들 수 있음

### 단락회로 평가

- AND(&&)의 경우 앞의 값이 false라면 뒤의 값과 관계없이 false가 되기 때문에 뒤의 값 처리를 생략가능함

- OR(||) 또한 앞의 값이 true라면 결과값이 true가 됨

- 따라서 어떤 객체가 있을 때 특정 프로퍼티를 넘기고 싶다면 `객체 && 객체.프로퍼티`로 작성하면 객체가 있을 때 객체의 프로퍼티가 결과값으로 남게 됨

- OR의 경우 값이 있을 때 앞의 값을 사용하고 없을 때 뒤의 값을 사용하도록 할 수 있음  `객체 || "객체가 없음"`

### 조건문 응용

- 여러 조건이 하나의 결과값을 가질 때 배열의 includes 메소드를 이용하여 나타낼 수 있음

    ```js
    if(["a", "b", "c", "d"].includes(type))
        return "type"
    ```

- 각 조건에 맞는 결과값을 가질 때 객체를 만들어 괄호 접근법으로 이용함

    ```js
    const option = {
        A : "a",
        B : "b",
        C : "c",
        D : "d",
    }

    ...

    return option[type] || "조건 없음";
    ```

### 동기, 비동기 

- 동기 방식
    - 코드가 순서대로 작업이 처리되는 것
    - 이전 작업이 진행중일 때는 다음 작업을 수행하지 않고 기다림
    - 블로킹 방식 사용
    - 문제점
        - 각 작업의 수행시간이 길다면 하나의 작업이 완료될 때 까지 전체가 기다려야하므로 전반적인 흐름이 느려짐

- 동기 방식을 멀티 쓰레드로 해결이 가능하나 자바스크립트는 싱글 쓰레드 방식을 사용

- 비동기 방식
    - 싱글 쓰레드 방식을 이용하면서, 동기적 작업의 단점을 극복하기 위해 여러개의 작업을 동시에 실행시킴
    - 먼저 작성된 코드 결과를 기다리지 않고 다음 코드를 바로 실행함
    - 논 블로킹 방식 사용

- setTimeout
    - 내장 비동기 타이머 함수
    - 파라미터1 : 콜백함수
    - 파라미터2 : 시간 (ms)

- JS Engine의 동기, 비동기 실행 방식
    - Heap 메모리 : 변수, 상수들의 사용되는 메모리 저장
    - CallStack : 작성한 코드의 실행에 따라서 작업을 쌓음
    ---
    - 동기 방식
        - JS Engine이 실행되면 CallStack에 Main Context가 가장 먼저 들어옴
        - 불러지는 순서대로 CallStack에 쌓이고 실행이 되었다면 쌓인 역순으로 빠져나가게 됨
        - 프로그램이 종료된다면 최종적으로 Main Context가 CallStack에서 빠져나가게 됨

    - 비동기 방식
        - 동기 방식에서 3가지가 추가되어 동작함(Web APIs, Callback Queue, Event Loop)
        - 동기 방식대로 호출된 함수들을 CallStack에 쌓이게 됨
        - 비동기 함수는 Stack될 때 콜백함수와 같이 Stack이 됨 이후 Web APIs로 넘겨짐
        - 넘겨진 비동기함수는 수행되고 수행이 완료 되었다면 비동기 함수의 Callback 함수가 Callback Queue로 옮겨짐
        - 이때 Event Loop는 CallStack에 Main Context를 제외한 다른 부분이 있는지 확인하고 없다면 Callback함수를 옮기게 됨

- Promise 사용시 Callback Hell 처럼 사용하는 것일 아니라 Promise를 반환하는 것을 유념하여 .then을 사용하면 됨

```js
function taskA(a, b){
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const res = a + b;
            resolve(res);
        }, 3000);
    });
}

function taskB(a){
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const res = a * 2;
            resolve(res);
        }, 1000);
    });
}

function taskC(a){
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const res = a * -1;
            resolve(res);
        }, 2000);
    });
}

taskA(5,1).then((a_res) => {
    console.log("A RESULT : ", a_res);
    return taskB(a_res);
}).then((b_res) => {
    console.log("B RESULT : ", b_res);
    return taskC(b_res);
}).then((c_res) => {
    console.log("C RESULT : ", c_res);
});

```

- 함수에 async를 붙이면 Promise 객체가 됨

- await를 Promise 객체에 붙이게 되면 해당 부분은 동기적 방식으로 처리


<br>