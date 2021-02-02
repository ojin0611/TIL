# CSS

> Cascading Style Sheet

스타일, 레이아웃 등을 통해 HTML이 사용자에게 어떻게 표시 되는지를 지정하는 언어

사용자에게 문서(HTML)를 표시하는 방법을 지정하는 언어



## CSS 구문

- 선택자와 함께 열린다.
- 스타일을 지정할 html 요소 선택
- 중괄호에 속성:값 쌍의 형태 선언

속성(Property) : 글꼴, 너비, 배경색 등 변경할 스타일 기능

값(Value) : 어떻게 변경할지

```css
.big-box {
  position: relative;
  margin: 100px auto 500px;
  border: 5px solid black;
  width: 500px;
  height: 500px;
}

```

## CSS 정의 방법

1. `Inline style`
2. 내부 참조 (`Embedding style`)
3. 외부 참조 (`Link style`)



# CSS 선택자

> 스타일을 지정할 웹 페이지의 HTML 요소를 선택



## 클래스 선택자

`.` 문자로 시작. 해당 클래스가 적용된 문서의 모든 항목 선택



## 아이디(id) 선택자

`#` 문자로 시작. 아이디는 문서당 한번만 사용 가능. 요소에는 단일 id값만 적용할 수 있다.



## 결합자

자손 결합자 (`A 공백 B `) : A의 모든 후손 요소(level n) 중 B와 일치하는 요소

자식 결합자 (`A > B`) : A의 모든 자식 요소(level 1) 중 B와 일치하는 요소

일반 형제 결합자 (`A ~ B`) : A의 형제요소 중 A 뒤에 위치하는 B요소 모두 선택

인접 형제 결합자 (`A + B`) : A의 형제요소 중 A 바로 뒤에 위치하는 B요소. 중간에 다른 요소 있으면 선택X



## 선택자 적용 우선순위

`!important` > inline style > id 선택자 > class 선택자 > 요소 선택자(h1, p) > 소스 순서

# CSS 단위

## 크기

px : 픽셀. 고정 단위

% : 백분율 단위

em : 상속의 영향을 받는다. 상황에 따라 각기 다른 값 가짐.

```css
font-size: 1.5em; /*상위 요소 크기의 1.5배*/
```

rem : 최상위 요소인 html을 기준으로 절대 단위. 상속 영향 X. 보통 16px

viewport : 현재 보이는 웹 컨텐츠 영역 기준. 1vh = viewport 높이의 1%. 1vw = viewport 너비의 1%



## 색상

색상 키워드 : red, blue, black

RGB : r,g,b,alpha(투명도)

HSL : 색상,채도,명도,alpha(투명도)



# Box Model

모든 HTML 요소는 box 형태로 돼있다. 하나의 박스는 네 영역으로 이루어진다.

> content / padding / border / margin

![img](images/4XNqriWaTAqdsIo7hIWq)

Content : tag가 존재하는 공간. 글이나 이미지, 비디오 등 요소의 실제 내용

Padding : Border(테두리) 안쪽의 **내부 여백**. 배경색, 이미지 지정 가능

Border

Margin : 테두리 바깥의 **외부 여백**. 배경색 지정 불가능

마진 상쇄 : block의 top, bottom margin이 때로는 가장 큰 하나의 마진으로 결합(상쇄, collapsed)된다.



# Display

> 요소를 블록과 인라인 요소 중 어느쪽으로 처리할지와 함께, 자식 요소를 배치할 때 사용할 레이아웃 설정

## block 

가로 영역을 모두 채운다. block 요소 다음 태그는 **줄바꿈**이 된다.

```css
<style>
.block1{ width: 300px; border: 3px solid #333 }
.block2{ width: 200px; border: 3px solid #999 }
</style>

<div class="block1">1</div>
<div class="block2">2</div>
```



## inline 

인라인 박스. 줄바꿈이 되지 않는다. 글자나 문장에 효과를 주기 위해 존재하는 단위.

width, height는 지정 불가능하다.

```css
<style>
.inline1{
	width: 200px; /* 이 값은 무시됩니다 */
	border: 3px solid #999;
}
</style>
<span class="inline1">reprehenderit</span>
```



## inline-block 

block과 inline의 중간 형태. 줄바꿈이 되지 않지만 크기를 지정할 수 있다.

```css
<style>
.inline-block2{
	display: inline-block;
	width: 200px; /* 이 값은 이제 정상 작동합니다 */
	border: 3px solid #999;
}
</style>
<span class="inline-block2">reprehenderit</span>
```





none : 화면에서 사라지고, 요소의 공간조차 사라짐. (`visibility:hidden;` 은 공간이 남음)



# Position

> 박스위 위치 속성 & 값
>
> static / absolute / relative / fixed
>
> z-index



## static

기본 위치. 모든 태그의 기본. 태그의 default



## relative

상대 위치. 기본 위치(static)를 기준으로 좌표 속성을 사용해 위치 이동

```html
<style>
.top {
  position: relative;
  top: 5px;
}
</style>
<span class="top">top</span>
```

relative는 각 방향을 기준으로 태그 안쪽방향으로 이동한다. 즉, 위의 top은 아래로 이동한다.



## absolute

절대 위치. static이 아닌 **부모/조상** 요소를 기준으로 좌표 속성만큼 이동

position이 relative, absolute, fixed인 부모가 없으면 body의 기준으로 이동한다.

```html
<style>
#parent {
  position: relative;
  width: 100px;
  height: 100px;
  background: skyblue;
}

#child {
  position: absolute;
  right: 0;
}
</style>

<div id="parent">
  <div id="child">children</div>
</div>
```



## fixed 

고정 위치. 브라우저의 viewport를 기준으로 좌표속성만큼 이동. 스크롤에 영향을 받지 않는다.