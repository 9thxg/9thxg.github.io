---
title: "[React.js] json-server DELETE 요청 문제"
author: 9uTae
date: 2022-02-09 17:30:00 UTC+09:00
categories: [WebPrograming, React.js]
tags: [react, React.js, json-server, JSON]
---

## json-server DELETE 요청 문제

### 문제점

![json-server-problem](https://user-images.githubusercontent.com/65030854/153194432-f490fca7-0e4f-4127-a240-22f132b51bb4.JPG)

- voca프로젝트 중에 Day를 삭제하는 과정에서 기존 마지막에 있는 Day를 삭제만 했지만 만약 마지막 Day에 단어들이 있다면 Day만 삭제되고 단어들만 남게되어 새로 Day추가시에 단어를 추가하지 않아도 나타나게 됨

- 위와 같은 이유로 Day 삭제전에 단어들을 먼저 삭제하도록 만들었음, 문제는 단어가 3~4개 쯤이면 json-server가 정상적으로 처리했지만 단어들이 많아지면 처리중에 json-server가 충돌이 생겨 종료되는 문제 발생

### 해결과정

- fetch가 비동기처리이지만 서버에 요청을 보낼 때 동시에 요청이 들어갔다고 생각되어 단어를 지우는 함수를 따로만들어 Promise객체를 반환하게 하고 forEach로 순회시 async await로 순차적으로 처리하도록 만듬

    - 위 해결방법으로 해결이 되지않아 Promise.all()을 사용하여 처리

    - 두 방법 모두 여러번 테스트하였을 때 모든 단어를 지우기도 했지만 비슷한 수로 오류가 발생했음
    
    ```js
    const DelWord = (wordId: number) => {
        return new Promise((resolve) => {
            fetch(`http://localhost:3002/words/${wordId}`, {
                method : 'DELETE',
            }).then((res) => {
                resolve(res);
            })
        })
    }

    function Delete() {
        if(window.confirm("정말 삭제하시겠습니까?(내용도 함께 사라집니다.)")){
            ...
            else{
                words.forEach(async (word) => await DelWord(word.id));

                Promise.all([words.map(async (word) => await DelWord(word.id))])
                .then((res) => {
                    console.log(res);
                })
            }
        }
    }
    ```

- json-server의 로그를 보았을 때 json-server는 db 파일에 변경이 생기면 reloading 하는 과정을 거침 위와 같이 실행시켰다면 앞선 요청에 의해 db파일에 변경사항이 생기고 json-server는 이를 처리하기 위해 reloading을 함 이때 요청이 들어오게 되면 오류가 발생하고 서버가 종료됨
    - json-server의 처리방식에 문제가 있기 때문에 구글링을 통해 한번의 요청으로 여러 데이터를 삭제하는 방법을 검색
    - 비슷한 케이스로 [글이](https://github.com/typicode/json-server/issues/860) 있었고 해결방안은 json-server 모듈 내부의 `plural.js`파일에서 DELETE하는 부분을 수정하여 여러 데이터를 삭제하도록 만듬

    ```js
    // 파일경로 : \AppData\Roaming\npm\node_modules\json-server\lib\server\router\plural.js
    // 기존의 코드
    function destroy(req, res, next) {
        let resource;

        if (opts._isFake) {
        resource = db.get(name).value();
        } else {
        resource = db.get(name).removeById(req.params.id).value(); // Remove dependents documents

        const removable = db._.getRemovable(db.getState(), opts);

        removable.forEach(item => {
            db.get(item.name).removeById(item.id).value();
        });
        }

        if (resource) {
        res.locals.data = {};
        }

        next();
    }

    // 수정한 코드
    function destroy(req, res, next) {
        const idList = req.params.id
        .split(',')
        .filter(item => item !== '' || item !== undefined || item !== null)
        function destroyById(id) {
        const resource = db
            .get(name)
            .removeById(id)
            .value()

        // Remove dependents documents
        const removable = db._.getRemovable(db.getState(), opts)
        removable.forEach(item => {
            db.get(item.name)
            .removeById(item.id)
            .value()
        })
        return resource
        }
        let resourceList = idList.map(destroyById)
        // if has one more id matched -> will not return 404
        if (resourceList.length >= 1 && resourceList.some(item => item)) {
        res.locals.data = {}
        }

        next()
    }

    // 출처 : https://github.com/Moon-Land/json-server/blob/release/src/server/router/plural.js
    ```

### 마무리

- json-server 모듈 파일을 수정하고 변경된 사용법으로 파일을 적용하여 해결하였음
- 이때 words에 받아온 데이터를 map을 통해 id값만 wordId에 저장하고 요청을 보낼 때 `.toString()` 메소드를 통해 문자열로 변경하여 요청이 처리되도록 만듬
- 최종적으로 DelWord 함수가 정상적으로 요청을 처리하게되면 DelDay 함수가 실행되도록 만듬

    ```js
    const DelWord = () => {
        const wordId = words.map(word => word.id);

        return (
            fetch(`http://localhost:3002/words/${wordId.toString()}`, {
                method : 'DELETE',
            })
        )
    }

    function Delete() {
        if(window.confirm("정말 삭제하시겠습니까?(내용도 함께 사라집니다.)")){
            if(words.length === 0){
                DelDay(days.length);
            }
            else{
                DelWord()
                .then(res => {
                    if(res.ok){
                        DelDay(days.length);
                    }
                })
            }
        }
    }
    ```