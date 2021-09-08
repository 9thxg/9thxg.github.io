---
title: "[Vue.js] Props 전달 및 Route 데이터 전달"
author: 9uTae
date: 2021-08-24 17:10:00 UTC+09:00
categories: [WebPrograming, Vue.js]
tags: [Vue.js, Vue, Vue-Cli, Props, Vue-Router]
---

## Vue.js Props 전달 및 Route 데이터 전달

<br>

> Vuetify를 통해 쇼핑몰 만드는 중에 Props를 전달하는 부분과 Route를 통해 데이터를 전달해야하는 부분이 있어 어떻게 전달하면 되는지 알아볼려고 한다.

<br>

---

### 1. 프로젝트 구성
- 현재 작성중인 프로젝트는 다음과 같이 구성되어있다.

    ![project](https://user-images.githubusercontent.com/65030854/130572138-8e736520-f5cf-45e6-974d-7a36b1ead650.PNG)

- `Item-card 컴포넌트`를 `Item-list` 컴포넌트에서 `v-for와 props전달`로 여러개의 객체로 만들어 준다.

- `Item-card 컴포넌트`는 `route`를 통해 해당 카드를 클릭하게 되면 `Detail-item 페이지`로 이동하게 되고 이때 `route`를 통해서 데이터(Params)도 함께 전달한다.

    ```vue
    <!--Item-card.vue-->
    <template>
        <router-link :to="{name: 'Item', params: { number: this.number}}" style=text-decoration:none;>
            <v-card elevation="2">
                <v-img height="250" />
                <v-divider></v-divider>
                <v-card-title>Number : {{number}}</v-card-title>
            </v-card>
        </router-link>
    </template>

    <script>
    export default {
        props: {
            number: {
                type: Number,
                require:true,
            }
        }
    }
    </script>
    ```

    ```vue
    <!--Item-list.vue-->
    <template>
        <div class="cover">
            <div v-for='n in 10' :key='n'>
                <ItemCard :number='n' />
            </div>
        </div>
    </template>

    <script>
    import ItemCard from "./Item-card.vue"

    export default {
        components: { ItemCard },
        data() {
            return{
                    
            }
        }
    }
    </script>
    ```

<br>

### 2. Props 전달
- Props는 부모 컴포넌트에서 자식 컴포넌트로 전달하는 값 또는 객체이다.
- 부모 컴포넌트에서 Props를 자식 컴포넌트로 전달하기 위해선 단순히 자식 컴포넌트를 `import`하는 것 뿐만아니라 `<script>`부분에 `components`등록까지 해주어야한다.

    ```vue
    <script>
        import ItemCard from "./Item-card.vue"

        export default {
            components: { ItemCard },
            ...
        }
    </script>
    ```

- Props를 전달하기 위해서 `v-bind`를 통해 데이터 바인딩해서 전달할 수 있다. `v-bind`는 생략이 가능하다.
    
    ```vue
    <ItemCard v-bind:number='n' />

    or 

    <ItemCard :number='n' />
    ```

- 받은 Props 값은 단지 읽기만 가능하다.(`readonly`)

- Props는 `array`형태 또는 객체형태로 작성이 가능하다.

    ```vue
    props: ['name', 'price']

    or

    props: {
        name: {},
        price: {}
    }
    ```

- `array`형태를 사용하게되면 고차원형태의 작업을 하기 힘들다. 따라서 주로 객체형태로 작성하는 것이 용이하다.
- 객체형태로 작성시에는 `props`의 값의 타입과 기본값, 필수조건을 설정할 수 있다.

    ```vue
    props: {
        name: {
            type: String,
            required: true,
            default: 'none'
        }
        price: {
            type: Number,
            default: 0
        }
    }
    ```


    __type:__ 변수타입 지정, 다른 타입이 들어오게 되면 에러가 발생하지만 렌더링은 됨

    __required:__ true일 경우 해당 값이 부모가 전달해 주지 않았을 때 에러발생, 렌더링은 됨

    **default:** 만약 값을 부모가 전달해 주지 않았을 때의 기본값 설정

- 만약 여러개의 `props`를 전달하게 될 경우에 `v-bind`문법을 이용하여 다같이 보낼 수 있다.
    
    ```vue
    따로 보내는 경우
    <ItemCard :name='a' :price=10000 />

    v-bind로 다같이 보냄
    <ItemCard v-bind="{name:'a', price:10000}" />
    ```

<br>

### 3. Route 데이터 전달
- Route를 통해서 데이터를 전달하는 방법은 `query`와 `params` 두 가지 방법이 있다.
- `params`의 경우 router폴더 index.js에서 `props:true`를 설정해주게 되면 props에도 데이터가 전달된다.

    ```js
    const routes = [
    {
        path: '/item',
        name: 'Item',
        props: true, // props 설정
        component: () => import('../views/Detail-item.vue')
    },
    ]
    ```

- 전달하는 방식은 태그에서 선언하는 부분과 함수에서 부분이 있다.
    ```js
    태그에서 전달
    <router-link :to="{name: '라우터 지정한 컴포넌트이름', query: {데이터1: '값', 데이터2: 값}}">
    // params경우 query 대신 params 입력

    함수에서 전달
    clickLink () {
                this.$router.push({name: '라우터 지정한 컴포넌트이름', query: {데이터1: '값', 데이터2: 값}})
            }
    // params경우 query 대신 params 입력
    ```

- 받은 데이터를 사용하기 위해 `<script>`부분에 `name`값과 `params`에서 `props`를 설정한 경우 `props`를 작성한다.

    ```js
    <script>
        export default {
            name: '컴포넌트 이름',
            props: {
                데이터1: {
                    type: String,
                    default : ''
                },
                데이터2: {
                    type: Number,
                    default: 0
                }
            }
        }
    </script>
    ```

- `query`또는 `params`로 받은 값을 바로 사용할 수도 있고 `params`에서 `props`를 설정한 경우 `props`로도 사용가능하다.

    ```html
    // 그대로 사용하는 경우
    <h2>데이터1: {% raw %}{{ $route.query.데이터1 }}{% endraw %}</h2>
    <h2>데이터2: {% raw %}{{ $route.params.데이터2 }}{% endraw %}</h2>

    // props로 사용하는 경우(params)
    <h2>데이터1: {% raw %}{{ 데이터1 }}{% endraw %}</h2>
    <h2>데이터2: {% raw %}{{ 데이터2 }}{% endraw %}</h2>
    ```

<br>

---

<br>

props전달과 route데이터 전달에 대해 알아보았다. 예제에서는 간단한 데이터로 전달했지만 많은 데이터들을 객체형태로 보낼 수 있다. 페이지를 구성하는데 자주 사용할 것 같다.

<br>