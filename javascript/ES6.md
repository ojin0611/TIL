# ES6

Javascript ES6의 특별한 기능들을 다룬다.

[TOC]

# let, const

let 예약어는 한번 선언한 똑같은 변수를 재선언할 수 없다.

const 예약어는 한번 할당한 값을 변경할 수 없다. 이 때, 객체나 배열을 할당하면 객체와 배열의 주소를 할당한 것이기 때문에 내부 값들을 변경해준다.

```javascript
// const 변수 배열 내부 값 변경
const num = [];
console.log(num); // []
num.push(10);
console.log(num); // [10]

// 호이스팅에서 제외.
var j = 10;
for (let j = 0; j < 5; j++) {
    // let 변수 j의 유효범위가 {}로 제한
    console.log(j);
}
console.log('j = ' + j);

```



# Property Shorthand

JS 객체를 정의할 때 객체의 key값과 value 값이 같으면, 각각 표기하지 않고 한 번만 표기

```javascript
const id = 'myid',
      name = '이름',
      age = 3;
const user = {
    id,
    name,
    age,
};
console.log(user);

```



# Concise method

객체의 value가 함수인 경우, key를 함수 형태로 만들면 `function()`을 생략할 수 있다.

```javascript
// info: function () {
//     console.log('something');
// }
// 위를 아래처럼 바꿀 수 있다.
info() {
    console.log('something');
},

```



# Destructuring Assignment

배열이나 객체에 입력된 값을 개별적인 변수에 할당하는 간편한 방식을 제공한다.

```javascript
const user = {
    id: 'myid',
    name: '김이름',
    age: 25,
};

let { id, name, age } = user;
console.log("hello " + id, name, age);
```



# Spread Syntax

`...`을 이용해 array를 전개할 수 있다. 

```javascript
// Spread Syntax (전개 구문)
const user1 = { id: "myid1" };
const user2 = { id: "myid2" };

// array copy : spread operator의 경우 값 복사가 아닌 주소를 갖고온다. 
const array = [user1, user2];
const arrayCopy = [...array];
console.log(array, arrayCopy); // [{id:myid5}, {id:myid2}]

// 호이스팅 발생! 
// 값을 바꿀 경우 배열 2개의 값 모두 변경
user1.id = "myid5";
console.log(array, arrayCopy); // [{id:myid5}, {id:myid2}]


```

전개된 배열 또는 객체를 병합할 수 있다.

```javascript
// array concat
const team1 = ["대전", "광주"];
const team2 = ["구미", "서울"];
const team = [...team1, ...team2];
console.log(team);

// obejct merge(병합)
const u1 = { id1: "myid1" };
const u2 = { id2: "myid2" };
const u = { ...u1, ...u2 };
console.log(u);

// 주의점 : key가 같은 obejct를 병합 하게 되면 마지막 obj가 기존 값을 덮어쓴다.
const user = { ...user1, ...user2 }; // 위의 코드
console.log(user); // {id: "myid2"}

```



# Default Parameter

다른 언어에도 있는 기능이다.

```javascript
function print2(msg = 'initial msg') {
    console.log(msg);
}
print2('hello');
print2(); // initial msg. default 없으면 undefined

```



# Template String

백틱을 이용하면 문자열을 보다 편리하게 표현할 수 있다. 엔터도 반영된다.

```javascript
// 기존
const id = 'myid1',
      name = '이름',
      age = 30;
// console.log(name + '(' + id + ')님 \n나이는 ' + age + '입니다.');
console.log(`${name}(${id})님 
나이는 ${age}입니다.`);

```



# Arrow Function

화살표를 이용해 익명함수를 표현한다

```javascript
const func3 = function (num) {
    console.log('익명 함수', num);
};
func3(3);

const func4 = (num) => {
    console.log('화살표 함수', num);
};
func4(4);
```

