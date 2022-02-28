---
title: "[React.js] Node.js, React.js 기초"
author: 9uTae
date: 2022-02-25 17:30:00 UTC+09:00
categories: [WebPrograming, React.js]
tags: [react, React.js, Node.js, node]
---

## Node.js, React.js 기초

<br>

---

<br>

### Node.js

- 자바스크립트 실행 환경

- 모듈 파일을 만들고 `module.exports` 객체를 통해 내보내기를 할 수 있음

  - 지정시에 함수 이름과 모듈 이름을 지정할 수 있음
  - 모듈을 사용하는 파일은 require 키워드를 통해 모듈 객체를 받아옴
  - 해당 방법은 Common JS 모듈 시스템임 따라서 바닐라 JS에선 불가능함

- npm (Node Package Manager)

  - Node.js 패키지 관리 도구
  - 패키지: Node.js의 모듈

- 패키지는 주로 root폴더를 만들어서 관리

- 패키지 생성

  - npm init 명령어를 입력하면 패키지 생성
  - 패키지명, 버전, 설명 등을 설정
  - 자동으로 해당 폴더에 package.json파일이 생성됨
    - package.json 파일은 패키지의 정보가 입력된 환경설정 파일임
    - main: 진입파일, 패키지에서 실행시킬 파일
    - scripts: 패키지를 개발하면서 자주 실행시킬 명령어 지정 `명령어명: 프롬프트에 실행될 명령어`
    - scripts의 명령어는 `npm 명령어명`로 실행

- 외부 패키지 사용
  - 다른 패키지를 다운받을려면 `npmjs.com`에서 찾아 사용
  - 패키지 설치시 package.json 기준으로 설치됨
  - 버전 왼쪽에 `^` 표시는 해당 버전 이상으로 설치된다는 뜻
  - node_modules 폴더에 외부 패키지 파일이 생성됨
  - pakage-lock.json 파일은 설치된 패키지들이 명시되어 있고 사용되고 있는 패키지들의 정확한 버전을 확인할 수 있음

<br>

### React.js

- component 기반의 UI 라이브러리

- 장점

  - 선언형 프로그래밍
  - Virtual DOM 사용
    - DOM(Document Object Model): 브라우저가 실제로 사용하는 객체

- React.js로 웹프로그래밍 시 추가적으로 필요한 라이브러리

  - Webpack: 모듈 번들 라이브러리, 다수의 자바스크립트 파일을 하나의 파일로 합침
  - Babel: JSX 등의 자바스크립트 문법을 사용할 수 있도록 해주는 라이브러리

- create-react-app

  - React.js로 웹프로그래밍을 시작하기 위해 세팅이 완료된 패키지
    - 이미 세팅이 완료된 패키지를 Boiler Plate라 함
  - npx로 create-react-app 실행
    - npx는 설치되어 있지 않은 라이브러리를 한 번 사용하고 버릴 때 사용

- React는 ES 모듈 시스템 사용

  - ES 모듈 시스템을 사용하지만 직접 환경을 구축해야할 때 Node.js는 Common JS모듈 시스템을 알아야 구축가능함

- Component
  - 데이터를 받지 못해도 정상적으로 출력해야할 때
    - `컴포넌트명.defualtProps = {}`를 통해 props를 전달받지 못해도 기본값으로 출력되도록 함
  - 부모 컴포넌트의 state가 변경되어 리렌더되면 자식 또한 리렌더 됨
  - 컴포넌트 자체도 props로 보낼 수 있음
    - props의 값은 컴포넌트 태그내에 놓아주면 컴포넌트 자체가 넘어감
    - `props.children`통해 사용할 수 있음

<br>
