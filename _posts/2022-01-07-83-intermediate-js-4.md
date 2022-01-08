---
title: "[JavaScript] 자바스크립트 중급4"
author: 9uTae
date: 2022-01-07 17:30:00 UTC+09:00
categories: [WebPrograming, JavaScript]
tags: [JavaScript, JS, 자바스크립트]
---

## 자바스크립트(JavaScript) 중급 4

<br>

---

<br>

### 클래스

- ES6에 추가된 스펙

- 생성자 함수와 class는 할당하는 방법은 같다

- 객체의 메소드가 생성자함수는 그 객체가 가지고 있지만 class는 프로토타입 내에 메소드가 존재

- 생성자 함수는 new가 없다면 오류구문없이 실행되고 undefined가 반환되지만 클래스는 new가 없다면 오류구문이 나타남

- for...in으로 순회하면 생성자 함수는 메소드를 포함하여 모든 프로퍼티를 보여주지만 class의 경우 메소드는 보여지지 않음

```js
// 생성자함수 사용
const User = function (name, age) {
    this.name = name;
    this.age = age;
    this.showName = function () {
        console.log(this.name);
    };
};

const mike = new User("Mike", 30);

//클래스 사용
class User2{
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    showName() {
        console.log(this.name);
    }
}

const tom = new User2("Tom", 19);
```

- class의 상속은 extends를 사용
    - 다른 class에서 class 상속(java와 유사)

- 부모 class 접근하기 위해 super 키워드 사용

```js
class Car {
    constructor(color){
        this.color = color;
        this.wheels = 4;
    }
    drive() {
        console.log("drive..");
    }
    stop() {
        console.log("STOP!");
    }
}

class Bmw extends Car {
    park(){
        console.log("PARK");
    }
    // 부모 class와 같은 프로퍼티가 있다면 자식 프로퍼티를 덮어씌움
    stop() {
        // 자식 class에서 부모 class의 함수를 불러오기위 해 super.stop()사용
        super.stop();
        console.log("OFF");
    }
}
```

- 생성자 오버라이딩
    - 자식 class에서 생성자를 만든다면 항상 부모 생성자를 호출해야함

    - 부모 class에서 생성자는 빈 객체를 만들어 this가 빈객체를 할당하고 가리키는 부분이 있지만 상속받은 자식 class는 빈 객체를 만들어 this를 할당하지 않음 따라서 항상 super 키워드를 통해 부모클래스의 생성자를 호출 해주어야 함

    - 부모 class의 생성자에 매개변수가 있따면 super키워드로 생성자 호출시 매개변수를 지정해주어야 정상 작동

```js
class Car {
    constructor(color){
        this.color = color;
        this.wheels = 4;
    }
    drive() {
        console.log("drive..");
    }
    stop() {
        console.log("STOP!");
    }
}

class Bmw extends Car {
    // 생성자 없이 함수만 있다면 자동을 아래와 같이 적용됨
    // constructor(...args){
    //     super(...args);
    // }
    constructor(color){
        super(color);
        this.navigation = 1;
    }
    park(){
        console.log("PARK");
    }
}
```

<br>

### Promise

- 요청에 대해서 준비되었다면 준비에 대한 콜백함수를 실패했다면 실패에 대한 콜백함수를 호출하는 방식
    - 콜백(callback)함수 : 어떤 일이 완료된 후 실행되는 함수

```js
const pr = new Promise((resolve, reject) => {
    //code
});
```

- `new Promise`가 반환한 객체는 state와 result의 프로퍼티를 가짐
    - state는 초기에 pending(대기) 상태이고 result값은 undefined를 가짐

- 준비되어서 resolve(value)가 호출되었다면 state는 fullfilled(이행됨) 상태가 되고 result값은 value가 된다

- 실패해서 reject(error)가 호출되면 state는 rejected(거부됨)이 되고 result는 error의 값을 가짐

```js
const pr = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('OK')
    }, 3000)
});

// then을 통해 resolve와 reject를 처리할 수 있음
pr.then(
    function(result){},
    function(err){}
)

// catch를 통해 다음과 같이 작성 가능
// 가독성이 더 좋고 첫번째 함수를 실행했다가 나는 에러를 잡아줌
pr.then(
    function(result){}
).catch(
    function(err){}
)

// finally도 추가할 수 있음 
// finally는 이행이든 거부든 처리가 완료되면 실행되는 부분(로딩화면을 없앨때 유용하게 사용)
pr.then(
    function(result){}
).catch(
    function(err){}
).finally(
    function(){
        console.log('---주문끝---')
    }
)
```

```js
// 3가지 주문이 있는 상황
const f1 = (callback) => {
    setTimeout(function() {
        console.log("1번 주문 완료");
        callback();
    }, 1000);
};

const f2 = (callback) => {
    setTimeout(function() {
        console.log("2번 주문 완료");
        callback();
    }, 3000);
};

const f3 = (callback) => {
    setTimeout(function() {
        console.log("3번 주문 완료");
        callback();
    }, 2000);
};

// depth가 깊어지면서 계속 콜백을 호출
// callback hell 이라 부름
f1(function() {
    f2(function() {
        f3(function(){
            console.log("끝");
        });
    });
});

/*----------------------------*/
// Promise를 사용하여 3가지 주문처리
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

// Promise를 사용하여 작업 처리
// Promise를 계속해서 이어 처리하는 것을 프로미스 체이닝(Promise chaining)이라고함
// 순차적으로 작업을 처리하기 때문에 총 6초가 걸림
f1()
.then((res) => f2(res))
.then((res) => f3(res))
.then((res) => console.log(res));
.catch(console.log)
.finally( => {
    console.log("끝");
});

/*----------------------------*/
// 세 작업을 동시에 처리
// Promise.all
// 인자는 배열로 받음
// 세 작업이 모두 완료하여야 .then 부분이 실행됨
// 해당 예시에선 3초에 모든 작업이 끝나기 때문에 총 3초가 걸림
// 단, 작업 중간에 reject시 실패 오류가 뜨고 어떤 데이터도 얻지 못함(하나의 정보라도 누락되면 페이지를 보여주면 안되는 경우 사용할 수 있음)
Promise.all([f1(), f2(), f3()]).then((res) => {
    console.log(res);
}

// Promise.race
// Promise.all 과 사용법은 동일함
// 세 작업 중 가장 먼저 완료된 작업기준으로 끝냄
// 용량이 큰 이미지를 로딩하는데 그 중에 하나라도 완료되면 그 이미지를 보여줄 때 사용할 수 있음
Promise.race([f1(), f2(), f3()]).then((res) => {
    console.log(res);
}
```

<br>
