# Vue router

라우팅은 웹 페이지 간 이동 방법으로, vue-router는 Vue.js의 공식 라우터다. [링크](https://router.vuejs.org/kr/)

라우터는 컴포넌트와 매핑해 Vue를 이용한 SPA를 제작할 때 유용하다.

URL에 따라 컴포넌트를 연결하고 설정된 컴포넌트를 보여준다.



## 설치

CDN

```html
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
```

NPM

```
npm install vue-router
```


## 연결

VueRouter의 routes 옵션을 이용한다. path에 따라 어떤 component를 보여줄지 결정한다.

```javascript
// 라우터 객체 생성
const router = new VueRouter({
  routes: [
    {
      path: '/',
      component: Main, // 컴포넌트를 담은 변수명
    },
    {
      path: '/board',
      component: Board, // 컴포넌트를 담은 변수명
    },
    {
      path: '/qna',
      component: QnA, // 컴포넌트를 담은 변수명
    },
    {
      path: '/gallery',
      component: Gallery, // 컴포넌트를 담은 변수명
    },
  ],
});

// Vue 객체에 인스턴트 라우터 주입
const app = new Vue({
  el: '#app',
  router,
});
```



## 이동 및 렌더링

`router-link` 컴포넌트를 사용한다. 속성은 to prop을 사용한다. 기본적으로 `<router-link to=>`는 `<a href=>` 태그로 렌더링된다.

```html
<p>
    <router-link to="/">HOME</router-link>
    <router-link to="/board">게시판</router-link>
    <router-link to="/qna">QnA</router-link>
    <router-link to="/gallery">갤러리</router-link>
</p>
```

현재 라우트에 맞는 컴포넌트가 `<router-view>`가 위치한 곳에 렌더링된다.

```html
<router-view></router-view>
```



## 라우터의 정보 접근

this.$router , this.$route를 이용해 전체, 현재 호출된 라우터 정보에 접근할 수 있다.

이를 이용해 url에 있는 parameter에도 접근할 수 있다.

```javascript
// 라우터 객체 생성
const router = new VueRouter({
    routes: [
        {
            path: '/board/:no',
            component: BoardView,
        },
    ],
});

const BoardView = {
    template: `<div>
{{no}}번 게시물 상세정보
<router-link to="/board">목록</router-link>
</div>`,
    data() {
        return {
            no: 0,
        };
    },
    created() {
        console.dir(this.$router); // 라우터 전체의 정보
        console.dir(this.$route); // 현재 호출된 해당 라우터 정보
        console.log(this.$route.params.no);
        console.log(this.$route.path);
        this.no = this.$route.params.no;
    },
}
```



## 이름을 갖는 라우트

라우터에 `name` 옵션을 사용해 이름이 있는 라우트를 사용할 수 있다.

```javascript
const router = new VueRouter({
    routes: {
        path: '/board',
        name: 'board',
        component: Board,
    }
}
```



 `router-link :to`로 보낼 때 name을 이용해 라우트를 지정해줄 수 있다.

```html
<router-link :to="{name: 'board'}">게시판</router-link>
```



## 프로그래밍 방식 라우터

라우터의 instance method를 이용해 프로그래밍으로 라우팅할 수 있다. 즉, `<router-link>`를 사용해 선언적 네비게이션용 anchor 태그를 만드는 것 외에도 라우터를 만드는 것이 가능하다.

```html
<a href="#board" @click="$router.push('/board')">게시판</a>
```



## 중첩된 라우트

일반적으로 앱 UI는 여러 단계로 중첩된 컴포넌트 구조다. URL의 세그먼트가 중첩된 컴포넌트의 특정 구조와 일치하는 것을 활용한다.

### 사용

1. redirect로 보내줄 컴포넌트의 url을 지정한다. board의 router-view에는 board/list에 해당하는 컴포넌트가 위치한다.

2. children 옵션을 이용해 board 컴포넌트의 자식 컴포넌트들을 추가해준다. 이 안에서는 자유롭게 사용이 가능하다.

```javascript
const router = new VueRouter({
    mode: 'history',
    routes: [
        {
            path: '/',
            name: 'main',
            component: Main,
        },
        {
            path: '/board',
            name: 'board',
            component: Board,
            redirect: '/board/list',
            children: [
                {
                    path: 'list',
                    name: 'boardlist',
                    component: BoardList,
                },
                {
                    path: 'write',
                    name: 'boardwrite',
                    component: BoardWrite,
                },
                {
                    path: 'detail/:no',
                    name: 'boardview',
                    component: BoardView,
                },
                {
                    path: 'update/:no',
                    name: 'boardupdate',
                    component: BoardUpdate,
                },
            ],
        },

    ]
}
```



