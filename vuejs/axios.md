# axios

Vue에서 권고하는 HTTP 통신 라이브러리로, Promise 기반의 HTTP 통신 라이브러리다.

상대적으로 다른 HTTP 라이브러리들에 비해 [문서화](https://github.com/axios/axios)가 잘 돼 있고, API가 다양하다.



## 설치

CDN : 

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```



NPM : `npm install axios`



## axios API

axios.get, axios.post, axios({옵션 속성}) 설정 등이 있다. [링크](https://github.com/axios/axios#axios-api)



**config**

```javascript
// POST
axios({
    method: 'post',
    url: '/user',
    data: {
        name: '홍길동',
        userid: 'ssafy'
    }
});
```



**GET** 

```javascript
const axios = require('axios');

// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) { // try
    // handle success
    console.log(response);
  })
  .catch(function (error) { // catch
    // handle error
    console.log(error);
  })
  .then(function () { // finally
    // always executed
  });

axios('/board/10'); // axios() default는 get이다.
```



**Delete**

`axios.delete(url[, config])`를 비롯한 3가지 방법이 존재한다.

```javascript
// 방법 1
axios({
    method:'delete',
    url: '/board/10'
});

// 방법 2
axios('/board/10', {
    method: 'delete'
});

// 방법 3
axios.delete('/board/10');
```



위를 이용해 axios로 기존의 ajax 코드를 대체하면 Spring으로 구성한 REST API와 통신이 가능해진다.