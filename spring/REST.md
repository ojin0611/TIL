# REST

Representational State Transfer의 약어로, URI는 하나의 고유한 리소스(Resource)를 대표하도록 설계된다는 개념에 전송방식을 결합해 원하는 작업을 지정한다.

웹의 장점을 최대한 활용할 수 있는 아키텍처로서, HHTP URI를 통해 자원을 명시하고, HTTP Method(GET, POST, PUT, DELETE)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍쳐다.

## 구성

자원(Resource) - URI

행위(Verb) - HTTP Method

표현(Representation of resource) - 파일 형태. json/xml/text

## 특징

데이터 처리만 하거나, 처리 후 반환될 데이터가 있으면 json/xml 형식으로 전달하기때문에 View에 대해서는 신경 쓸 필요가 없다.

정해진 표준이 없지만 암묵적인 표준은 다음과 같다.

- URI는 명사를 사용한다.
- 하이픈은 사용하지만 언더바는 사용하지 않는다.
- 대문자 사용은 하지 않는다.
- 확장자가 포함된 파일 이름을 URI에 사용하지 않는다.
- 슬래시(`/`)로 계층관계를 나타낸다.
- URI 마지막에 슬래시(`/`)를 사용하지 않는다.



# REST API

## Jackson library

`jackson-databind` 라이브러리는 객체를 json 포맷의 문자열로 변환시켜 브라우저로 전송한다.

`jackson=dataformat-xml` 라이브러리는 객체를 xml 포맷으로 변환해 브라우저로 전송한다.

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>${jackson-databind-version}</version>
</dependency>
```



## Annotation

`@RestController` : Controller가 REST 방식을 처리하기 위한 것임을 명시한다.

`@ResponseBody` : 데이터 자체를 전달

`@PathVariable` : URI 경로에 있는 값을 파라미터로 추출

`@CrossOrigin` : Ajax의 크로스 도메인 문제를 해결

`@RequestBody` : JSON 데이터를 원하는 타입으로 바인딩

```java
// userid에 해당하는 user get
@GetMapping(value = "/user/{userid}")
public ResponseEntity<MemberDto> userInfo(@PathVariable("userid") String userid) {
    logger.debug("파라미터 : {}", userid);
    MemberDto memberDto = userService.userInfo(userid);
    if(memberDto != null)
        return new ResponseEntity<MemberDto>(memberDto, HttpStatus.OK);
    else
        return new ResponseEntity(HttpStatus.NO_CONTENT);
}

```

[ResponseEntity란](https://devlog-wjdrbs96.tistory.com/182)

```javascript
// double click으로 user 정보 읽기
$(document).on("dblclick", "tr.view", function() {
    let vid = $(this).attr("data-id");
    $.ajax({
        url:'${root}/admin/user/' + vid,  
        type:'GET',
        contentType:'application/json;charset=utf-8',
        success:function(user) {
            $("#vid").text(user.userid);
            $("#vname").text(user.username);
            $("#vemail").text(user.email);
            $("#vaddress").text(user.address);
            $("#vjoindate").text(user.joindate);
            $("#userViewModal").modal();
        },
        error:function(xhr,status,msg){
            console.log("상태값 : " + status + " Http에러메시지 : "+msg);
        }
    });			
});
```

