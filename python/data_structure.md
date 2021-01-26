# 데이터 구조
데이터 구조(Data Structure)란 데이터에 편리하게 접근하고, 변경하기 위해서 데이터를 저장하거나 조작하는 방법을 말한다.

> Program = Data Structure + Algorithm



순서

- 알고리즘에 빈번히 활용되는 순서가 있는(ordered) 데이터 구조 
    - 문자열(String)
    - 리스트(List)
- 알고리즘에 빈번히 활용되는 순서가 없는(unordered) 데이터 구조 
    - 세트(Set)
    - 딕셔너리(Dictionary)
- 데이터 구조에 적용 가능한 Built-in Function



## 데이터의 분류

데이터는 크게 변경 가능한 것(`mutable`)들과 변경 불가능한 것(`immutable`)으로 나뉘며, python은 각각을 다르게 다룹니다.

### 변경 불가능한(`immutable`) 데이터

* 리터럴(literal)

    - 숫자(Number)
    - 글자(String)
    - 참/거짓(Bool)

* range()

* tuple()

* frozenset()



### 변경 가능한(`mutable`) 데이터

- `list`
- `dict`
- `set`



# 문자열

> 변경할 수 없고(immutable), 순서가 있고(ordered), 순회 가능한(iterable)



## 조회 / 탐색

`.find(x)`

`.index(x)`



## 값 변경

`.replace(old, new[,count])`

`.strip([chars])`

`.split()`

`'separator'.join(iterable)`

## 문자 변형

`.capitalize()`, `.title()`, `.upper()` ,`.lower()`

`.swapcase()`



# 리스트

> 변경 가능하고(mutable), 순서가 있고(ordered), 순회 가능한(iterable)



## 값 추가 및 삭제

`append(x)`, `.extend(iterable)`, `.insert(i,x)`

`.remove(x)`, `.pop(i)`

`.clear()`



## 탐색 및 정렬

`.index(x)`

`.count(x)`

`.sort()`

`.reverse()`



## 리스트 복사

1. slice 연산자
2. deepcopy



## List Comprehension

List Comprehension은 표현식과 제어문을 통해 리스트를 생성합니다.

여러 줄의 코드를 한 줄로 줄일 수 있습니다.

---

### 활용법

```python
[expression for 변수 in iterable]

list(expression for 변수 in iterable)

[expression for 변수 in iterable if 조건식]

[expression if 조건식 else 식 for 변수 in iterable]
```



# 세트 (Set)

> 변경 가능하고(mutable), 순서가 없고(unordered), 순회 가능한(iterable)



## 추가 및 삭제

`.add(elem)`

`.update(*others)`

`.remove(elem)`, `discard(elem)`  : KeyError 유,무

`pop()`



# 딕셔너리(Dictionary)

> 변경 가능하고(mutable), 순서가 없고(unordered), 순회 가능한(iterable)
>
> `Key: Value` 페어(pair)의 자료구조



## 조회

`.get(key[, default])`



## 추가 및 삭제

`.update()`

`.pop(key[, default])`



## Dictionary Comprehension

### 활용법
`iterable`에서 `dict`를 생성할 수 있습니다.

```python
{키: 값 for 요소 in iterable}

dict({키: 값 for 요소 in iterable})

{키: 값 for 요소 in iterable if 조건식}

{키: 값 if 조건식 else 값 for 요소 in iterable}

```







# 데이터 구조와 자주 사용되는 Built-in Function

## `map(function, iterable)`

* 순회가능한 데이터 구조(iterable)의 모든 요소에 function을 적용한 후 그 결과를 돌려준다. 


* `map_object` 형태를 반환한다.

```python
def cube(n):
    return n ** 3

list(map(cube, [1,2,3]))
# [1, 8, 27]
```



## `filter(function, iterable)`

* iterable에서 function의 반환된 결과가 `True` 인 것들만 구성하여 반환한다.


* `filter object` 를 반환한다.



```python
def odd(n):
    return n % 2

list(filter(odd, [1,2,3]))
# [1, 3]
```



## `zip(*iterables)`

* 복수의 iterable 객체를 모아(`zip()`)준다.


* 결과는 튜플의 모음으로 구성된 `zip object` 를 반환한다.
* `*` : unpack 연산자

```python
students = [
 [100, 80, 100],
 [90, 90, 60],
 [80, 80, 80]
]

list(zip(*students))
# [(100, 90, 80), (80, 90, 80), (100, 60, 80)]
```

