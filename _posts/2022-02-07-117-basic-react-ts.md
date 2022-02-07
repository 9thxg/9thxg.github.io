---
title: "[React.js] React.js TypeScript"
author: 9uTae
date: 2022-02-07 17:30:00 UTC+09:00
categories: [WebPrograming, React.js]
tags: [react, React.js, TypeScript, typescript]
---

## React.js TypeScript 기초

<br>

---

<br>

### TypeScript

- TypeScript는 정적 타입을 지원하여 명확한 코드 기술과 가독성을 높여 개발속도를 향상 시킬 수 있음
    - 프로퍼티 확인이 쉬움
        - 오타나 없는 프로퍼티를 쓰는 실수를 줄임
    - 프로퍼티 타입에서 사용할 수 있는 메소드 확인 가능

- 설치
    - 프로젝트 초기에 도입하는 것이 좋음
    - 진행된 뒤에 적용하기 위해서 프로젝트에 적용된 웹팩들에 대해 types를 지정하여 설치해야함

    ```cmd
    npm install typescript @types/node @types/react @types/react-dom @types/jset @types/react-router-dom
    ```
    - create-react-app으로 설치시 node, react, react-dom, jest는 필수로 필요
    - 추가한 웹팩 패키지를 추가적으로 적용해야함

- js확장자 파일은 ts로 변경하고 jsx파일은 tsx로 변경

- interface 선언

    ```js
    export interface ITypename{
        property1?: string;
        property2?: number;
        property3?: boolean;
        data: any;
        data2: IData;
    }
    ```

    - any 타입은 되도록 사용하지 않도록 함
        - 모든 타입이 적용되기 때문에 typescript 의미가 사라짐
    - 프로퍼티를 optional하게 만들려면 프로퍼티명 뒤에 "?"를 입력
         - optional 하기 때문에 데이터 입력시 필수적으로 들어가지 않아도 됨
         - 통상 모든 값이 필요하다고하면 optional은 쓰지 않도록 하는것이 좋음
    - 다른파일에서도 interface를 사용해야한다면 앞에 export를 붙임

- 필요한 변수 뒤에 클론과 인터페이스명을 기입
    - `datas: ITypename[]`

- 기타 타입
    - useParams 경우 가져온 객체에 필요한 데이터가 있는지 확실치 않은 경우
        - `<>`제네릭을 사용하여 `<{ 데이터명: 데이터타입 }>` 형식으로 입력
    - event 객체의 경우 상황에 맞는 적합한 event 타입을 지정해줘야 함
        - 많은 event 타입이 있기 때문에 검색 필수
    - Ref의 current는 렌더링 후에도 없을 수 있기 때문에 null인지 체크 후 사용하는 것이 좋음

<br>