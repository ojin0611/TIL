# Promise

[TOC]



[참고 : CAPTAIN PANGYO](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/#%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4-%EC%BD%94%EB%93%9C---%EA%B8%B0%EC%B4%88)

프로미스는 자바스크립트 비동기 처리에 사용되는 객체다. 프로미스는 주로 서버에서 받아온 데이터를 화면에 표시할 때 사용한다. 

```javascript
// callback 적용
function getData(callbackFunc) {
  $.get('url 주소/products/1', function(response) {
    callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌.
      // 데이터가 완전히 받아진 후에 getData 함수가 끝난다.
  });
}

getData(function(tableData) {
  console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});

// promise 적용
function getData(callback) {
  // new Promise() 추가
  return new Promise(function(resolve, reject) {
    $.get('url 주소/products/1', function(response) {
      // 데이터를 받으면 resolve() 호출
      resolve(response);
    });
  });
}

// getData()의 실행이 끝나면 호출되는 then()
getData().then(function(tableData) {
  // resolve()의 결과 값이 여기로 전달됨
  console.log(tableData); // $.get()의 reponse 값이 tableData에 전달됨
});

```



## Promise의 3가지 상태

프로미스의 상태(states)란 프로미스의 처리 과정을 의미한다. `new Promise()`로 프로미스를 생성하고 종료될 때까지 3가지 상태를 갖는다.

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
- Fulfilled(이행) : 비동기 처리가 완료돼 프로미스가 결과값을 반환해준 상태
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

![img](images/promise.svg) 

**Pending**

`new Promise()` 메서드를 호출하면 대기(Pending) 상태가 된다. 메서드 호출 시 콜백함수를 선언할 수 있고, 콜백함수의 인자는 `resolve`, `reject`다.

```javascript
new Promise(function(resolve, reject){
    // ...
});
```

**Fulfilled**

콜백 함수의 인자 `resolve`를 아래와 같이 실행하면 이행(Fulfilled) 상태가 된다.

```javascript
new Promise(function(resolve, reject){
    resolve();
});
```

이행 상태가 되면 아래와 같이 `then()`을 이용해 처리 결과 값을 받을 수 있다.

```javascript
function getData(){
    return new Promise(function(resolve, reject){
        var data = 100;
        resolve(data);
    })
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function(resolvedData) {
  console.log(resolvedData); // 100
});
```

**Rejected**

`new Promise()`로 프로미스 객체를 생성하면 콜백 함수 인자로 resolve와 reject를 사용할 수 있는데, reject를 아래처럼 호출하면 실패(Rejected) 상태가 된다.

```javascript
new Promise(function(resolve, reject){
    reject();
});
```

실패 상태가 되면 실패한 이유(실패 처리의 결과값)를 `catch()`로 받을 수 있다.

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값 Error를 err에 받음
getData().then().catch(function(err) {
  console.log(err); // Error: Request is failed
});
```

