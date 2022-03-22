---
title: "[React.js] 한입 크기로 잘라 먹는 React.js 정리"
author: 9uTae
date: 2022-03-22 17:30:00 UTC+09:00
categories: [WebPrograming, React.js]
tags: [react, React.js]
---

## 한입 크기로 잘라 먹는 React.js 정리

<br>

---

<br>

### 컴포넌트 props

- 컴포넌트에서 props를 정상적으로 받지 못해도 출력하고 싶다면 defaultProps 사용

  ```js
  컴포넌트명.defaultProps = {};
  ```

  - 내용에 props 기본값을 입력해주면 됨

- 부모 컴포넌트의 state가 변경되어 리렌더 된다면 자식 컴포넌트 또한 리렌더됨

- 컴포넌트 자체도 props로 넘겨줄 수 있음
  - props의 값을 컴포넌트 태그 내의 놓아주면 컴포넌트 자체가 넘어가짐
  - 이를 children으로 접근 가능함

### useRef

- DOM을 조작할 수 있게 하는 React Hook

- useRef 객체를 만들고 해당 태그에 ref로 추가
- current로 해당 태그 접근 가능
- `focus()` 메소드를 호출하면 해당 태그가 선택된(집중된) 상태가 됨
  - 주로 `alert()`를 통해 사용자에게 알려주는 때가 많지만 UX 관점에서 좋지 못한 경험이기 때문에 `focus()`를 사용하여 알려줌
    - `focus()`시 좀 더 알아보기 쉽도록 CSS설정해주면 좋을 것 같음

### List 출력

- prop을 이용하여 리스트 출력

  - key 값을 고유한 값으로 지정 (주로 id값)
  - 값을 넘길때 전개구문 사용
  - JSX로 작성할 때 재사용과 가독성을 위하여 컴포넌트로 따로 만드는 것이 좋음

- Date 객체

  - Date 객체 안에 ms 값을 넣고 `.toLocalString()` 메소드를 호출하면 우리가 알아보기 쉬운 날짜 값이 출력됨

  ```js
  new Date(created_date).toLocalString();
  ```

### 컴포넌트 구조

- 컴포넌트 구조를 그려(트리형식) 데이터 흐름이나 페이지의 전체적인 구조를 확인하는 것이 좋음

- 같은 레벨의 컴포넌트 끼리 데이터를 주고 받을 수 없기 때문에 상위 컴포넌트에서 데이터 state를 만들어 데이터가 입력되는 부분과 setData를 출력하는 부분을 통해 data를 주고 받을 수 있음

- id값을 자동으로 하나씩 늘려가며 지정해줄 때 useRef(0)를 사용하여 지정

  ```js
  dataId = useRef(0);

  ...
  {
      id: dataId.current
  }

  dataId.current += 1;
  ```

  - useRef가 이때 가리키고 있는값은 0임 따라서 current를 입력하면 0을 가리키게 됨

- 자식 컴포넌트에게 부모 컴포넌트의 props를 전달하기 위해 사용하지 않고 넘겨주기만 하는 것을 `props drilling` 이라고 함

- 삼항연산자를 통해서 컴포넌트가 state의 상태에 따라 보이게 할 수 있음

  ```js
  <div>
    {isEdit ? (
      <>
        <button onClick={handleQuitEdit}>수정 취소</button>
        <button onClick={handleEdit}>수정 완료</button>
      </>
    ) : (
      <>
        <button onClick={handleRemove}>삭제</button>
        <button onClick={toggleIsEdit}>수정</button>
      </>
    )}
  </div>
  ```

- 기존값과 다르게 변경할 값이 필요하다면 따로 state를 만들어 관리
- 객체 값을 수정할때는 map과 전개구문을 적절히 활용

### LifeCycle

- 생성: Mount, 변화: Update, 종료: Unmount

- LifeCycle과 관련된 메소드들은 class형 컴포넌트에만 사용할 수 있음
- class형 사용하던 기능을 앞에 `use`를 붙여 함수형 컴포넌트에서 사용할 수 있도록 하는것을 `Hooks`라 부름
  - class형 컴포넌트는 중복코드, 가독성문제, 코드길이가 길어지는 문제가 있음
  - `Hooks`기능은 2019년 6월에 출시
- useEffect

  - 첫번째 인자는 callback 함수이고 두번째 인자는 의존성 배열임
  - Mount 때 사용하고 싶다면 의존성 배열에 빈배열 입력
  - Update시 모든 state 변경에 반응한다면 두번째 인자 생략, 특정 state에 반응한다면 의존성 배열안에 해당 state입력
  - Unmount 때 사용하기 위해서 Mount 형식의 useEffect에서 callback 함수안에 return을 통해 callback 함수 정의

  ```js
  const UnmountTest = () => {
    useEffect(() => {
      console.log("Mount!");

      return () => {
        console.log("Unmount!");
      };
    }, []);

    return <div>Unmount Testing Component</div>;
  };
  ```
