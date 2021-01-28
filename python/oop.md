# 객체지향 프로그래밍

Object가 중심(oriented)이 되는 프로그래밍

**<wikipedia - 객체지향 프로그래밍>**
>
>객체 지향 프로그래밍(영어: Object-Oriented Programming, OOP)은 컴퓨터 프로그래밍의 패러다임의 하나이다. 
>
>객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다.

# 클래스

* 클래스 생성은 `class` 키워드와 정의하고자 하는 `<클래스의 이름>`으로 가능하다.
* `<클래스의 이름>`은 `PascalCase`로 정의한다.
* 클래스 내부에는 데이터와 함수를 정의할 수 있고, 이때 데이터는 **속성(attribute)** 정의된 함수는 **메서드(method)**로 불린다.
* **인스턴스**는 특정 class로부터 생성된 해당 클래스의 실체/예시다.

---

**활용법**

```python
class <클래스이름>:
    <statement>
```

```python
class ClassName:
    statement
```

### 생성자

```python
def __init__(self, name, gender):
    self.__name = name
    self.__gender = gender
```



### 소멸자

```python
def __del__(self):
    print('소멸자 호출')
```

### 매직 메서드

- 더블언더스코어(`__`)가 있는 메서드는 특별한 일을 하기 위해 만들어진 메서드이기 때문에 `스페셜 메서드` 혹은 `매직 메서드`라고 불립니다.
- 매직(스페셜) 메서드 형태: `__someting__`
```
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__rmod__',
 '__rmul__',
 '__setattr__',
 '__sizeof__',
 '__str__',
```

### `__str__(self)` 

```py
class Person:
    def __str__(self):
        return '객체 출력(print)시 보여줄 내용'
```

- 특정 객체를 출력(`print()`) 할 때 보여줄 내용을 정의할 수 있음



## 변수
### 인스턴스 변수

- 인스턴스의 속성
- 각 인스턴스들의 고유한 변수
- `self.varname` 으로 정의
- 인스턴스 생성 후, `인스턴스.변수명` 으로 접근 및 할당



#### 인스턴스 변수의 접근 제한

```python
class Person:
    self.__name = name
    # 이 경우, getter/setter 메소드 제공 여부에 대한 고민이 필요하다
```



### 클래스 변수

* 클래스의 속성(attribute)
* 모든 인스턴스가 공유
* 클래스 선언 내부에서 정의
* `클래스.변수명`으로 접근 및 할당

---

**활용법**
```py
class Person:
    species = 'human'
    
print(Person.species)
```





## 인스턴스 & 클래스간의 이름공간

### 이름공간 탐색 순서

* 클래스를 정의하면, 클래스가 생성됨과 동시에 이름 공간(namespace)이 생성된다. 


* 인스턴스를 만들게 되면, 인스턴스 객체가 생성되고 해당되는 이름 공간이 생성된다.


* 인스턴스의 어트리뷰트가 변경되면, 변경된 데이터를 인스턴스 객체 이름 공간에 저장한다.


* 즉, 인스턴스에서 특정한 어트리뷰트에 접근하게 되면 **인스턴스 => 클래스** 순으로 탐색을 한다.



> **데코레이터**
>
> @property 를 이용해 getter,setter 등 역할 가능
>
> ```python
> class Person:
>     @property
>     def name(self):
>         
>     @property
>     def age(self):
>         return self.__age
>     
>     @age.setter
>     def age(self, age):
>         if age<0:
>             raise TypeError("나이는 0 이상")
>         self.__age = age 
> ```



## 메소드

### 인스턴스 메소드

- 인스턴스가 사용할 메서드

* 클래스 내부에 정의되는 메서드의 기본값은 인스턴스 메서드
* **호출시, 첫번째 인자로 인스턴스 자기자신 `self`이 전달됨**

---

**활용법**

```python
class MyClass:
    def instance_method(self, arg1, arg2, ...):
        ...

my_instance = MyClass()
# 인스턴스 생성 후 메서드를 호출하면 자동으로 첫 번째 인자로 인스턴스(my_instance)가 들어갑니다.
my_instance.instance_method(.., ..)  
```



### 클래스 메소드

* 정적 메서드로, 인스턴스에서도 접근이 가능하다.
* 클래스가 사용할 메서드
* `@classmethod` 데코레이터를 사용하여 정의
* **호출시, 첫 번째 인자로 클래스 `cls`가 전달됨**
* 클래스 자체(cls)와 그 속성에 접근할 필요가 있을 때 사용한다.

---

**활용법**

```python
class MyClass:
    @classmethod
    def class_method(cls, arg1, arg2, ...):
        ...

# 자동으로 첫 번째 인자로 클래스(MyClass)가 들어간다.
MyClass.class_method(.., ..)  
```



### 스태틱 메서드(static method)
* 클래스가 사용할 메서드
* `@staticmethod` 데코레이터를 사용하여 정의
* **호출시, 어떠한 인자도 전달되지 않음**
* 클래스와 클래스 속성(데이터)에 접근할 필요가 없을 때 사용한다.


---

**활용법**

```python
class MyClass:
    @staticmethod
    def static_method(arg1, arg2, ...):
        ...

# 아무런 일도 자동으로 일어나지 않는다.
MyClass.static_method(.., ..)
```


### 오버로딩

비교연산자 오버로딩

![images/Untitled%208.png](images/Untitled%208.png)

![images/Untitled%209.png](images/Untitled%209.png)

이외에도 `__str()__`  를 오버로딩할 수 있다.




### 인스턴스와 메서드
- 인스턴스는 3가지 메서드 모두에 접근할 수 있다.
- 하지만 인스턴스에서 클래스 메서드와 스태틱 메서드는 호출하지 않아야 한다. (가능하다 != 사용한다)
- 인스턴스가 할 행동은 모두 인스턴스 메서드로 한정 지어서 설계한다.



### 클래스와 메서드
- 클래스 또한 3가지 메서드 모두에 접근할 수 있다.
- 하지만 클래스에서 인스턴스 메서드는 호출하지 않는다. (가능하다 != 사용한다)
- 클래스가 할 행동은 다음 원칙에 따라 설계한다. (클래스 메서드와 정적 메서드)
    - 클래스 자체(`cls`)와 그 속성에 접근할 필요가 있다면 **클래스 메서드**로 정의한다.
    - 클래스와 클래스 속성에 접근할 필요가 없다면 **정적 메서드**로 정의한다.
        - 정적 메서드는 `cls`, `self`와 같이 묵시적인 첫번째 인자를 받지 않기 때문





# 상속

클래스에서 상속이란, 물려주는 클래스(Parent Class, Super class)의 내용(속성과 메소드)을 물려받는 클래스(Child class, sub class)가 가지게 되는 것이다.

```python
class Person:
    def __init__(self, name='사람'):
        self.name = name

class Student(Person): # Student가 Person을 상속함
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
```



super() 를 이용하면 더 편하게 상속할 수 있다.

```python
class Student(Person):
    # 학생은 생성할 때, 학번을 추가로 받고 싶어요
    def __init__(self, name, student_id):
        super().__init__(name) # 여기가 실행되는 것은 부모클래스의 init()실행하고
        # 추가 작업
        self.student_id = student_id
```



## 메서드 오버라이딩

자식 클래스에서 부모 클래스의 메서드를 재정의하는 것

* 상속 받은 메서드를 `재정의`할 수도 있다. 
* 상속 받은 클래스에서 **같은 이름의 메서드**로 덮어쓴다.



## 상속관계에서의 이름공간

* 기존의 `인스턴스 -> 클래스` 순으로 이름 공간을 탐색해나가는 과정에서 상속관계에 있으면 아래와 같이 확장된다.
* 인스턴스 -> 클래스 -> 전역
* 인스턴스 -> 자식 클래스 -> 부모 클래스 -> 전역



## 다중 상속

두개 이상의 클래스를 상속받는 경우, 다중 상속이 된다.

```python
class FirstChild(Mom, Dad):
	...
```

위 경우 Mom, Dad의 속성 이름이 같을 때 Mom의 속성을 참조한다.

`mro` 메소드를 사용하면 상속의 순서에 따라 어떤 메서드를 실행할지 알 수 있다.

```python
FirstChild.mro()
# [__main__.FirstChild, __main__.Mom, __main__.Dad, __main__.Person, object]
```



