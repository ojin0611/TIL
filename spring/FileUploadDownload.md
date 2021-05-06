[TOC]

# File Upload

파일을 업로드하는 기능을 추가한다.



## 사용

### pom.xml

pom.xml에 common file upload를 DI해준다.

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
```



### servlet-context.xml

- bean을 추가해준다. (maxUploadSize, maxInMemorySize)

```xml
<!-- fileUpload -->
<beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <beans:property name="defaultEncoding" value="UTF-8"/>
    <beans:property name="maxUploadSize" value="52428800"/> 
    <beans:property name="maxInMemorySize" value="1048576"/> 
</beans:bean>

```



### upload

form tag를 사용한다.

- method="post", enctype="multipart/form-data"
- input type="file"

```html
<form id="writeform" method="post" enctype="multipart/form-data" action="">
    <div class="form-group" align="left">
        <label for="subject">파일:</label>
        <input type="file" class="form-control-file border" name="upfile" multiple="multiple">
    </div>    
</form>
```



### DTO

File 정보를 담은 DTO를 생성한다. 게시판에 파일 등록 기능을 추가한다면 게시판 DTO에 `List<FileInfoDto>`  타입의 변수를 추가해준다.

```java
public class FileInfoDto {
	private String saveFolder;
	private String originFile;
	private String saveFile;
}
```



### Controller

위의 form에서 `upfile`이라는 parameter로 들어오는 값을 `MultipartFile[] files`로 받는다. file 저장폴더, 원본이름, 저장되는이름, 저장되는 시간 등을 기록해 FileInfoDto에 담는다. 

파일이 저장되는 경로를 지정하는 부분도 Controller에 존재한다.

UUID를 이용해 중복되지않는 파일명을 만드는 로직도 아래 코드에 존재한다.

```java
@RequestMapping(value = "/write", method = RequestMethod.POST)
public String write(GuestBookDto guestBookDto, 
    		@RequestParam("upfile") MultipartFile[] files, 
    		Model model, HttpSession session) 
    	throws IllegalStateException, IOException {

    List<FileInfoDto> fileInfos = new ArrayList<FileInfoDto>();
    for (MultipartFile mfile : files) {
        FileInfoDto fileInfoDto = new FileInfoDto();
        String originalFileName = mfile.getOriginalFilename();
        if (!originalFileName.isEmpty()) {
            String saveFileName = UUID.randomUUID().toString() + originalFileName.substring(originalFileName.lastIndexOf('.'));
            fileInfoDto.setSaveFolder(today);
            fileInfoDto.setOriginFile(originalFileName);
            fileInfoDto.setSaveFile(saveFileName);
            logger.debug("원본 파일 이름 : {}, 실제 저장 파일 이름 : {}", mfile.getOriginalFilename(), saveFileName);
            mfile.transferTo(new File(folder, saveFileName));
        }
        fileInfos.add(fileInfoDto);
    }

}
```



Service

`@Transactional` annotation을 통해 글과 파일 모두 올라갔을때만 DB에 업로드한다.

```java
@Override
@Transactional
public void writeArticle(GuestBookDto guestBookDto) throws Exception {
    guestBookMapper.writeArticle(guestBookDto);
    if(guestBookDto.getFileInfos() != null) {
        guestBookMapper.fileRegist(guestBookDto);
    }
}
```



### mapper

xml에 여러 개의 파일을 등록하는 쿼리를 사용하기 위해 foreach 구문을 사용한다.

```xml-dtd
<insert id="writeArticle" parameterType="GuestBookDto">
	insert into guestbook (userid, subject, content, regtime)
	values (#{userid}, #{subject}, #{content}, now())
	<selectKey resultType="int" keyProperty="articleno" order="AFTER">
       	SELECT LAST_INSERT_ID()
   	</selectKey>
</insert>
	
<insert id="fileRegist" parameterType="GuestBookDto">
	insert into file_info (articleno, savefolder, originfile, savefile)
	values
	<foreach collection="fileInfos" item="fileinfo" separator=" , ">
	(#{articleno}, #{fileinfo.saveFolder}, #{fileinfo.originFile}, #{fileinfo.saveFile})
	</foreach>
</insert>
```



# File Download

## 사용

### servlet-context.xml

file download 시 return해줄 view로 등록할 bean을 설정한다. 즉, view name을 return할 때 jsp페이지가 아닌 특정 클래스를 실행한다.

```xml
<!-- fileDownload -->
<beans:bean id="fileDownLoadView" class="com.ssafy.guestbook.view.FileDownLoadView"/>
<beans:bean id="fileViewResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver">
    <beans:property name="order" value="0" />
</beans:bean> 

```



위처럼 설정하면 FileDownLoadView.java 파일에 파일 다운로드를 위한 클래스를 작성하여 다운로드를 처리할 수 있다.