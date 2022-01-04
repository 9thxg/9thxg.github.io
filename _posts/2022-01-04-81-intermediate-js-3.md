---
title: "[JavaScript] 자바스크립트 중급3"
author: 9uTae
date: 2022-01-03 17:30:00 UTC+09:00
categories: [WebPrograming, JavaScript]
tags: [JavaScript, JS, 자바스크립트]
---

## 자바스크립트(JavaScript) 중급 3

<br>

---

<br>

### call, apply, bind

- 함수 호출 방식과 관계없이 `this`를 지정할 수 있음

```js
const mike = {
    name: "Mike",
};

const tom = {
    name: "Tom",
};

function showThisName() {
    console.log(this.name);
}

// this가 가리킬 대상이 없으므로 window를 가리키고 있음
showThisName(); // 아무것도 뜨지않음

// call 메소드를 통해 this 지정
// call은 모든 함수에 사용할 수 있음
// this를 특정값으로 지정할 수 있음
showThisName.call(mike);
showThisName.call(tom);

// 객체에 birthYear과 occupation 프로퍼티를 추가하는 함수
function update(birthYear, occupation) {
    this.birthYear = birthYear;
    this.occupation = occupation;
};

// call을 통해 birthYear과 occupation 추가
update.call(mike, 1999, "singer");
update.call(tom, 2002, "teacher");

// apply 메소드는 call 동작하는 방식은 완전히 같음
// 단 매개변수 처리시 배열을 통해 처리
update.apply(mike, [1999, "singer"]);
update.apply(tom, [2002, "teacher"]);

// call과 apply 활용 예제
const nums = [3, 10, 1, 6, 4];
// const minNum = Math.min(...nums);
// const maxNum = Math.max(...nums);

// Math.min메소드는 this가 크게 상관없으므로 null값을 넣음(다른값도 상관없음)
// 배열로 매개변수를 받기 때문에 nums그대로 입력
const minNum = Math.min.apply(null, nums);

// call 메소드는 매개변수를 따로 받아야하므로 전개구문을 통해 배열을 나눠 입력
const maxNum = Math.max.call(null, ...nums);

// bind 메소드는 함수의 this값을 영구 히 바꿀 수 있음
// bind를 통해 mike를 this로 지정
const updateMike = update.bind(mike);

// 매개변수만 있지만 bind로 this를 지정했기 때문에 정상적으로 프로퍼티에 반영이 됨
updateMike(1980, "police");

// 다음과 같은 객체가 있는경우
const user = {
    name: "Mike",
    showName: function () {
        console.log(`hello, ${this.name}`);
    },
};

user.showName(); // hello, Mike

// fn이 객체의 함수프로퍼티를 할당 받음
let fn = user.showName;

// call, apply를 통해 this를 user로 지정
fn.call(user); // hello, Mike
fn.apply(uesr); // hello, Mike

// boundFn는 fn의 this를 user로 지정되고 할당받음
let boundFn = fn.bind(user);

boundFn(); // hello, Mike

```

<br>

### 상속, 프로토타입(prototype)

- 객체 생성시 생성한 프로퍼티 외에 `__proto__`가 존재함 이것을 프로토타입이라 부름

- 객체에 프로토타입에 있는 프로퍼티가 존재한다면 해당 프로퍼티가 호출되면 객체내의 프로퍼티에서 탐색이 멈춤
    - 만약 객체가 해당 프로퍼티를 가지고 있지 않다며 프로토타입까지 탐색

- 상속
    - 여러 객체의 공통된 부분을 프로토타입으로 해결가능

    - 공통된 프로퍼티를 가지는 상위 객체를 생성하고 각 객체의 프로토타입(`__proto__`)을 상위 객체로 지정

    - 프로토타입은 계속해서 연결할 수 있음(이어서 계속해서 상속할 수 있음)
        - 프로토타입 체인(Prototype Chain)이라 부름

    ```js
    const car = {
        wheels: 4,
        drive() {
            console.log("drive..");
        },
    };

    const bmw = {
        color: "red",
        navigation: 1,
    };

    // 상위객체 상속
    bmw.__proto__ = car;

    const x5 = {
        color: "white",
        name: "x5",
    };

    // 프로토타입 체인
    x5.__proto__ = bmw;
    ```

    - for...in을 통해 프로토타입체인이 된 객체를 순회하면 본인프로퍼티 포함 상속받음 프로퍼티까지 순회함
        - 만약 for...in에서 본인프로퍼티를 구분하고 싶다면 hasOwnProperty 메소드를 사용

    ```js
    for(p in x5){
        // 본인 포함 상속받은 모든 프로퍼티 순회
        // console.log(p);

        // 다음과 같이 구분가능
        if(x5.hasOwnProperty(p)){
            console.log('o', p);
        } else{
            console.log('x', p);
        }
    }
    ```

    - 생성자함수를 사용할 때 공통된 프로퍼티를 포함하는 상위객체를 만들어 상속해서 사용해도 되지만 번거로움이 있음
        - 생성자함수에 프로토타입에 프로퍼티를 지정하여 입력

    ```js
    //const car = {
    //    wheels: 4,
    //    drive() {
    //        console.log("drive..");
    //    },
    //};

    const Bmw = function (color) {
        this.color = color;
    };

    // 다음과 같이 생성자함수에 프로토타입에 프로퍼티를 추가 가능
    Bmw.prototype.wheels = 4;
    Bmw.prototype.drive = function () {
        console.log("drive..");
    }
    Bmw.prototype.navigation = 1;

    const x5 = new Bmw("red");
    const z4 = new Bmw("blue");

    //x5.__proto__ = car;
    //z4.__proto__ = car;
    ```

    - intanceof
        - 객체와 생성자를 비교가능
        - 프로토타입의 프로퍼티 중 constructor와 관련이 있음

    - 생성자함수의 프로토타입 프로퍼티를 수정할 때 객체를 통해 한꺼번에 수정하면 constructor프로퍼티가 사라짐
        - 따라서 프로퍼티를 하나씩 추가하거나 객체형식으로 작성한다면 명시적으로 constructor를 작성해주어야 함
        - 즉 JS는 명확한 생성자를 보장하지 않음(개발자에 의해 수정가능)

    ```js
    // constructor를 지정하지 않았을 경우 생성된 객체의 constructor 프로퍼티와 Bmw와 비교하였을 때 false의 결과가 나옴 (x5.constructor === Bmw)
    // 따라서 객체형식으로 프로토타입의 프로퍼티 입력시 constructor 프로퍼티를 명식적으로 작성
    Bmw.prototype = {
        constructor : Bmw,
        wheels : 4,
        drive () {
            console.log("drive..");
        },
        navigation : 1,
    }
    ```

    - 생성자 함수에서 변수를 은닉하기 위해 클로저를 이용하여 함수를 만들어 변수에 직접적으로 접근하지 못하게 할 수 있음
    
    ```js
    // 다음과 같이 클로저를 이용하여 은닉화 가능
    // 생성될 때의 color값을 직접 접근할 방법이 없음
    const Bmw = function(color){
        const c = color;
        this.getColor = function(){
            console.log(c);
        };
    };

    const x5 = new Bmw("red");
    ```

<br>
