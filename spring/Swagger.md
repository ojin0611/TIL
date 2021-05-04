# Swagger

Swagger는 간단한 설정으로 프로젝트의 API 목록을 웹에서 확인 및 테스트할 수 있게 해주는 Library다. Swagger를 사용하면 Controller에 정의돼있는 모든 url을 바로 확인할 수 있다.

API 목록뿐만 아니라 API의 명세 및 설명도 볼 수 있고, API를 직접 테스트해 볼 수도 있다.

FrontEnd 개발자의 경우 화면과 로직에 집중을 하고 BackEnd 개발자가 만든 문서 API를 보며 데이터 처리를 하게 된다. 이 때, API의 추가 또는 변경할 때마다 문서에 적용하는 불편함을 해결해준다. 



## 사용

pom.xml에 dependency 추가

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```



Swagger 적용한 Configuration class. `@Bean` 으로 등록한 메소드가 return하는 Docket이 API를 구성한다.

```java
@Configuration
@EnableSwagger2
public class SwaggerConfiguration{
    @Bean
    public Docket api(){
		List<ResponseMessage> responseMessages = new ArrayList<ResponseMessage>();
		responseMessages.add(new ResponseMessageBuilder().code(200).
                             message("OK!!!").build());
		responseMessages.add(new ResponseMessageBuilder().code(500).
                             message("서버 문제 발생!").responseModel(new ModelRef("Error")).build());
		responseMessages.add(new ResponseMessageBuilder().code(404).
                             message("페이지를 찾을 수 없습니다!").build());
		return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo()).groupName(version).select()
            .apis(RequestHandlerSelectors.basePackage("com.a.b.controller"))
            .paths(postPaths()).build()
            .useDefaultResponseMessages(false)
            .globalResponseMessage(RequestMethod.GET,responseMessages);
    }
	private Predicate<String> postPaths() {
		return regex("/admin/.*");
	}
    
   	// API에 대한 설명
	private ApiInfo apiInfo() {
		return new ApiInfoBuilder().title(title)
				.description("something") 
				.contact(new Contact("Name", "https://edu.url.com", "url@url.com"))
				.license("Some License")
				.licenseUrl("https://www.url.com/")
				.version("1.0").build();

	}

}
```



Swagger 적용될 Controller다.  `@Api` annotation을 이용해 API의 이름을 명시한다.

`@ApiOperation`는 각 API Method 위에 적는다. 

`@ApiParam`은 각 parameter 앞에 적는다.

```java
@RestController
@RequestMapping("/admin")
@Api("Admin Controller API V1")
@CrossOrigin("*")
public class AdminController {
	@ApiOperation(value="회원목록", notes="회원의 <big>전체목록</big>을 반환한다.")
	@ApiResponses({
		@ApiResponse(code=200, message="회원목록OK"),
		@ApiResponse(code=404, message="페이지 없어"),
		@ApiResponse(code=500, message="서버에러")
	})
	@GetMapping(value = "/user")
	public ResponseEntity<List<MemberDto>> userList() {
		List<MemberDto> list = userService.userList();
		if(list != null && !list.isEmpty()) {
			return new ResponseEntity<List<MemberDto>>(list, HttpStatus.OK);
		} else {
			return new ResponseEntity(HttpStatus.NO_CONTENT);
		}
	}
    
	@ApiOperation(value="회원삭제", notes="userid 해당하는 회원1명 삭제")
	@DeleteMapping(value = "/user/{userid}")
	public ResponseEntity<List<MemberDto>> userDelete(@PathVariable("userid") @ApiParam(value="삭제할 회원 아이디", required=true) String userid) {
		userService.userDelete(userid);
		List<MemberDto> list = userService.userList();
		return new ResponseEntity<List<MemberDto>>(list, HttpStatus.OK);
	}

}
```



Swagger가 적용될 Dto다.

```java
@ApiModel(value="MemberDto : 회원정보", description = "회원의 상세 정보를 나타냄")
public class MemberDto {

	@ApiModelProperty(value="회원 아이디")
	private String userid;
	@ApiModelProperty(value="회원 이름")
	private String username;
	@ApiModelProperty(value="회원 비밀번호")
	private String userpwd;
	@ApiModelProperty(value="회원 이메일")
	private String email;
	@ApiModelProperty(value="회원 주소")
	private String address;
	@ApiModelProperty(value="회원 가입일")
	private String joindate;
}
```



여기까지 설정하면 아래 URL에서 Swagger를 확인할 수 있다.

Swagger 설정 확인 : http://localhost:8000/{your-app-root}/v2/api-docs
Swagger-UI 확인 : http://localhost:8080/{your-app-root}/swagger-ui.html