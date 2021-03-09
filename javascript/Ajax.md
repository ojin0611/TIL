# Ajax

AJAX(Asynchronous Javascript And XML)는 구현하는 방식이다. 웹에서 화면을 갱신하지않고 데이터를 서버로부터 가져와 처리하는 방법이다. 

## 서버와 클라이언트

Javascript의 XMLHttpRequest(XHR) 객체로 클라이언트는 데이터를 전달하고 비동기방식으로 결과를 조회한다.

서버는 요청을 처리한 후 Text,XML, JSON,CSV 등의 데이터로 응답한다. 클라이언트는 이 데이터를 이용해 화면전환없이 현재 페이지에서 동적으로 화면을 재구성한다.

정리하면, 서버 중심의 개발방식은 화면 구성이 서버에서 이루어지고 (JSP, PHP), 클라이언트 중심 개발방식은 클라이언트(웹 브라우저)에서 화면을 구성한다. (주로 JS)

Ajax는 클라이언트 중심의 개발방식이며 비동기요청보다는 **동적화면 구성**이 관건이다.



## Javascript Ajax

XMLHttpRequest 객체가 갖고있는 메소드,속성들을 사용해 데이터를 주고받는다. 직접 만든 js 코드를 사용한다. (httpRequest 변수에 XMLHttpRequest() 객체 저장하고, 요청 주고받는 코드)

```javascript
// 서버의 시간을 출력하기
httpRequest = new XMLHttpRequest();
// --- 시간 가져오는 연산 --- //
if(httpRequest.readyState == 4){ // 0~3 : 대기. 4 : 데이터 일부 받음. 5: 데이터전부받음
    if(httpRequest.status==200){ // 200 : success. 404 : page not error
        var time = httpRequest.responseText;
        var div = document.getElementById("curtime");
        div.setAttribute("class", "viewtime");
        div.innerHTML= time;
    }
}
```



## jQuery Ajax

`$.ajax(options)` 함수는 jQuery에서 Ajax 기능을 제공하는 가장 기본적인 함수다.  옵션 속성으로는 주로 `url, data, type, success(data, status, xhr)`를 사용한다.

### GET / POST

`$.get(), $.post()` 함수는 위의 `$.ajax()` 의 옵션 속성중 type 옵션이 미리 지정된 함수다. 

**GET**

- URL에 데이터를 포함시켜 요청한다.
- 데이터를 Header에 포함하여 전송한다.
- URL에 데이터가 노출되어 보안에 취약하다.
- 전송하는 길이에 제한이 있다.
- 캐싱할 수 있다.

|          | GET                                | POST                          |
| -------- | ---------------------------------- | ----------------------------- |
| url      | URL에 data 포함시켜 요청           | URL에 data 노출하지 않고 요청 |
| data     | header에 포함한다                  | body에 포함한다               |
| 보안     | 취약하다. url에 데이터 노출        | 기본 보안은 된다.             |
| 전송길이 | 제한이 있다.                       | 전송 길이에 제한이 없다.      |
| 캐싱     | 가능하다. (레지스터에 데이터 저장) | 불가능하다.                   |



### load

서버로부터 내용을 조회해 선택자를 통해 탐색한 DOM 객체에 동적으로 삽입한다.

`$(selector).load(url);`

`$(selector).load(url, data, function(result, textStatus, jqXHR));`



### 데이터 전송 형식

대표적인 방식으로는 csv, xml, json 등이 있다.



### event 관리 (전역함수)

Ajax 전역함수를 이용해 ajax 처리중 진행상태를 보여주는 기능을 구현할 수 있다. `$.ajaxSetup()`함수에 global 프로퍼티 설정이 true인 경우에만 수행된다.(default=true)

```javascript
// 로딩화면 구현
$(document).ajaxStart(function() { // XHR 객체 생성 전에 실행
    $("#loading").fadeIn(); 
}).ajaxStop(function() { // 모든 Ajax 진행이 완료, 다른 전역콜백이 호출된 후
    $("#loading").hide();
});

```



