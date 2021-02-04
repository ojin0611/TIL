# Bootstrap

[Bootstrap Homepage](https://getbootstrap.com/)

부트스트랩은 이미 만들어진 모양, 편리한 기능들을 제공한다. html 요소들을 class로 조작한다. 

크로스 브라우징을 지원한다.

그리드 시스템을 지원한다.

반응형 웹 디자인을 추구한다.

**Bootstrap 홈페이지에서 검색해서 찾아서 쓰자**



## CDN

css는 head에 넣는다.

```html
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-giJF6kkoqNQ00vy+HMDP7azOuL0xtbfIcaT9wjKHr8RbDVddVHyTfAAsrekwKmP1" crossorigin="anonymous">  

```



javascript는 body 끝에 넣는다. 늦게 불러와져도 되니까.

```html
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.bundle.min.js" integrity="sha384-ygbV9kiqUc6oa4msXn9868pTtWMgiQaeYH7/t7LECLbyPA2x65Kgf80OJFdroafW" crossorigin="anonymous"></script>

```



## Grid System

container, row, column을 이용한다. 12개의 column 시스템을 가진다.

> .container > .row > col-*

Nesting(중첩)이 가능하다.

특정 px 조건(breakpoint)마다 크기를 지정할 수 있다. 

> None(<576px), sm(>=576), md(>=768), lg(>=992), xl(>=1200), xxl(>=1400)

무조건 **container부터 시작해야된다**.

`col-md-4` , `offset-2` 



## 자주 쓰이는 클래스

### spacing
M/P
- M : Margin
- P : Padding

t , b , l , r ,x , y
- t : top
- b : bottom
- l : left
- r : right
- x : x축 -> left , right
- y : y축 -> top , bottom

0, 1, 2, 3, 4, 5, auto
- 0 : 0
- 1 : .25rem( font-size가 16px이면, 4px) 크기
- 2 : .5rem( font-size가 16px이면, 8px) 크기
- 3 : 1rem( font-size가 16px이면, 16px) 크기
- 4 : 1.5rem( font-size가 16px이면, 24px) 크기
- 5 : 3rem( font-size가 16px이면, 48px) 크기
- auto : margin의 자동으로 세팅



### color

`bg-secondary` 회색, `bg-danger` 빨강

### text

`text-center` 텍스트 중앙정렬

`fw-bold` bold text

### display

`d-inline`, `d-block`

### position

`fixed-top`



### flex

적용한 요소의 바로 자식만 flex가 적용된다.

`d-flex` : flexbox 적용

`flex-column` : 위에서 아래로 쌓기

`justify-content-between` 가운데 간격 두고 띄우기



