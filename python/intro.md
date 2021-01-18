

# 변수

### 할당 연산자(Assignment Operator): `=`

* 변수는 `=`을 통해 할당(assignment) 된다. 

* 해당 데이터 타입을 확인하기 위해서는 `type()`을 활용한다.

* 해당 값의 메모리 주소를 확인하기 위해서는 `id()`를 활용한다.

### 식별자(Identifiers)

파이썬에서 식별자는 변수, 함수, 모듈, 클래스 등을 식별하는데 사용되는 이름(name)입니다. 

* 식별자의 이름은 영문알파벳(대문자와 소문자), 밑줄(_), 숫자로 구성된다.
* 첫 글자에 숫자가 올 수 없다.
* 길이에 제한이 없다.
* 대소문자(case)를 구별한다.
* 아래의 키워드는 사용할 수 없다. [파이썬 문서](https://docs.python.org/ko/3/reference/lexical_analysis.html#keywords)

```
# keyword.kwlist
False, None, True, and, as, assert, async, await, break, class, continue, def, del, elif, else, except, finally, for, from, global, if, import, in, is, lambda, nonlocal, not, or, pass, raise, return, try, while, with, yield
```



# 데이터 타입

## 숫자 타입

int, float, complex가 있다.

**파이썬에서 표현할 수 있는 가장 큰 수**
* 파이썬에서 가장 큰 숫자를 활용하기 위해 sys 모듈을 불러온다.

  * > sys.maxsize

* 파이썬은 기존 C 계열 프로그래밍 언어와 다르게 정수 자료형(integer)에서 오버플로우가 없다.

* 임의 정밀도 산술(arbitrary-precision arithmetic)을 사용하기 때문이다. 

> **오버플로우(overflow)**
- 데이터 타입 별로 사용할 수 있는 메모리의 크기가 제한되어 있다.
- 표현할 수 있는 수의 범위를 넘어가는 연산을 하게 되면, 기대했던 값이 출력되지 않는 현상, 즉 메모리가 차고 넘쳐 흐르는 현상

> **임의 정밀도 산술(arbitrary-precision arithmetic)**
- 사용할 수 있는 메모리양이 정해져 있는 기존의 방식과 달리, 현재 남아있는 만큼의 가용 메모리를 모두 수 표현에 끌어다 쓸 수 있는 형태
- 특정 값을 나타내는데 4바이트가 부족하다면 5바이트, 더 부족하면 6바이트까지 사용할 수 있게 유동적으로 운용



## 문자 타입

### 이스케이프 시퀀스

문자열을 활용하는 경우 특수문자 혹은 조작을 하기 위하여 사용되는 것으로 `\`를 활용하여 이를 구분합니다. 

| <center>예약문자</center> |    내용(의미)     |
| :-----------------------: | :---------------: |
|            \n             |      줄 바꿈      |
|            \t             |        탭         |
|            \r             |    캐리지리턴     |
|            \0             |     널(Null)      |
|           \\\\            |        `\`        |
|            \\'            | 단일인용부호(`'`) |
|            \\"            | 이중인용부호(`"`) |



### String interpolation 

* `%-formatting` 

    * `%d` : 정수
    
    * `%f` : 실수
    
    * `%s` : 문자열

* [`str.format()` ](https://pyformat.info/)

* [`f-strings`](https://www.python.org/dev/peps/pep-0498/) : 파이썬 3.6 이후 버전에서 지원



# 연산자

* 파이썬에서 and는 a가 거짓이면 a를 리턴하고, 참이면 b를 리턴한다.
* 파이썬에서 or은 a가 참이면 a를 리턴하고, 거짓이면 b를 리턴한다.

### 단축평가
* 첫 번째 값이 확실할 때, 두 번째 값은 확인 하지 않음
* 조건문에서 뒷 부분을 판단하지 않아도 되기 때문에 속도 향상



- `and` 는 둘 다 True일 경우만 True이기 때문에 첫번째 값이 True라도 두번째 값을 확인해야 하기 때문에 'b'가 반환된다.
- `or` 는 하나만 True라도 True이기 때문에 True를 만나면 해당 값을 바로 반환한다.

```python
'a' and 'b' # b
'a' or 'b'  # a
```



### Identity

`is` 연산자를 통해 동일한 object인지 확인할 수 있습니다. (OOP 파트에서 다시 학습하게 됩니다.)

파이썬에서 -5 부터 256 까지의 id는 동일합니다.  `a=5, b=5`

공백없는 알파벳 문자열도 id가 같다. `a = 'hi', b='hi'`



## Expression & Statement

### 표현식(Expression)

> 표현식 => `evaluate` => 값

* 하나의 값(value)으로 환원(reduce)될 수 있는 문장
* `식별자`, `값`(리터럴), `연산자`로 구성됩니다.
* 표현식을 만드는 문법(syntax)은 일반적인 (중위표기) 수식의 규칙과 유사합니다.

[파이썬 문서](https://docs.python.org/ko/3/reference/expressions.html)



### 문장(Statement)

* 파이썬이 실행 가능한 최소한의 코드 단위 (a syntatic unit of programming)



### 문장과 표현식의 관계

![statement_expression](https://user-images.githubusercontent.com/9452521/87619771-f41f5e00-c757-11ea-9e4b-1f76e4ca0981.png)



# 컨테이너

여러 개의 값을 저장할 수 있는 것(~~객체~~)

- 시퀀스(Sequence)형: 순서가 있는(ordered) 데이터
- 비 시퀀스(Non-sequence)형: 순서가 없는(unordered) 데이터



## 시퀀스(sequence)형 컨테이너

`시퀀스`는 데이터가 순서대로 나열된(ordered) 형식을 나타냅니다. 

* **주의! 순서대로 나열된 것이 `정렬되었다(sorted)`라는 뜻은 아니다.**

### 특징
1. 순서를 가질 수 있다.

2. **특정 위치의 데이터를 가리킬 수 있다.**

### 종류
파이썬에서 기본적인 시퀀스 타입은 다음과 같습니다.

* 리스트(list)

* 튜플(tuple)

* 레인지(range)

* *문자형(string)*


### 시퀀스에서 활용할 수 있는 연산자/함수 

|operation|설명|
|---------|---|
|x `in` s	|containment test|
|x `not in` s|containment test|
|s1 `+` s2|concatenation|
|s `*` n|n번만큼 반복하여 더하기
|`s[i]`|indexing|
|`s[i:j]`|slicing|
|`s[i:j:k`]|k간격으로 slicing|
|len(s)|길이|
|min(s)|최솟값|
|max(s)|최댓값|
|s.count(x)|x의 개수|

## 비 시퀀스형(Non-sequence) 컨테이너

- 셋(set)

- 딕셔너리(dictionary)

## `set`

`set`은 순서가 없는 자료구조입니다.

* `set`은 수학에서의 집합과 동일하게 처리된다. 

* `set`은 중괄호`{}`를 통해 만들며, 순서가 없고 중복된 값이 없다.

* 빈 집합을 만들려면 `set()`을 사용해야 합니다. (`{}`로 사용 불가능.)

### 활용법
```python
{value1, value2, value3}
```

## `dictionary`

`dictionary`는 `key`와 `value`가 쌍으로 이뤄져있으며, 궁극의 자료구조이다. 

### 활용법

```python
{Key1:Value1, Key2:Value2, Key3:Value3, ...}
```

* `{}`를 통해 만들며, `dict()`로 만들 수도 있다.
* `key`는 **변경 불가능(immutable)한 데이터**만 가능하다. (immutable : string, integer, float, boolean, tuple, range)
* `value`는 `list`, `dictionary`를 포함한 모든 것이 가능하다.



## 컨테이너형 형변환

파이썬에서 컨테이너는 서로 변환할 수 있습니다.

![type-casting](https://user-images.githubusercontent.com/18046097/61180466-a6a67780-a651-11e9-8c0a-adb9e1ee04de.png)



## 데이터의 분류
> `mutable` vs. `immutable`

데이터는 크게 변경 가능한 것(`mutable`)들과 변경 불가능한 것(`immutable`)으로 나뉘며, python은 각각을 다르게 다룹니다.



### 변경 불가능한(`immutable`) 데이터

* 리터럴(literal)

    - 숫자(Number)
    - 글자(String)
    - 참/거짓(Bool)

* range()

* tuple()



[immutable 데이터 실습](http://pythontutor.com/iframe-embed.html#code=a%20%3D%2020%0Ab%20%3D%20a%0Ab%20%3D%2010%0A%0Aprint%28a%29%0Aprint%28b%29&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=0&heapPrimitives=nevernest&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false)



### 변경 가능한(`mutable`) 데이터

- `list`
- `dict`
- `set`



[mutable 데이터 실습](http://pythontutor.com/iframe-embed.html#code=num1%20%3D%20%5B1,%202,%203,%204%5D%0Anum2%20%3D%20num1%0Anum2%5B0%5D%20%3D%20100%0A%0Aprint%28num1%29%0Aprint%28num2%29&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=0&heapPrimitives=nevernest&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false)



## 컨테이너(Container) 정리
<center><img src="https://user-images.githubusercontent.com/18046097/61180439-44e60d80-a651-11e9-9adc-e60fa57c2165.png", alt="container"/></center>