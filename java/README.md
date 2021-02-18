[TOC]

# 대입 연산자

## 대입 연산자의 리턴값

```java
int a = 0;
System.out.println(a=3);
```

위의 코드를 실행하면 3이 출력된다. 즉, 대입 연산자에는 리턴값이 존재한다.

이를 이용해 LinkedList의 다음 노드를 계속 탐색할 수 있다.



```java
while ((nextNode = currNode.link) != null) { // nextNode = currNode가 가리키는 노드
    if (nextNode == target) { // 다음 노드가 target이라면
        return currNode; // 현재 노드 리턴
    }
    currNode = nextNode;
}

```



# 자바의 메모리구조

자바는 메모리를 크게 3개의 공간으로 나누어 사용한다. 실제로는 더 있는데, 이정도만 알고 넘어간다.

1. 메소드 영역
2. 힙 영역
3. 스택 영역



## 표

| 영역   | 내용                                                         | 생성                        | 소멸            |
| ------ | ------------------------------------------------------------ | --------------------------- | --------------- |
| method | global variable, method, static variable                     | 컴파일된 클래스파일 실행 시 | 프로그램 종료   |
| heap   | class object, array object, String object, new 연산자를 사용 | new                         | 자동 소멸 by GC |
| stack  | local variable, parameter                                    | 블럭 안에서 선언 시         | 블럭이 닫히면   |



## 예제

```java
class Test {
    int a; // 객체 생성 시, heap에 저장
    static int b; // 컴파일 시 method 영역에 저장
    
    public void view(int k){ // method 자체는 바이트 형태로 method 영역에 저장
        int s; // parameter(k)와 함께 stack에 저장
    } // method 호출 종료 시, s와 k 소멸
    
    public static void main(){
        Test ob = new Test(); // 객체는 heap에 저장
        ob = new Test(); // 이전에 생성된 객체는 추후 Garbage Collector에 의해 소멸
        ob.a = 10; // 객체의 값 heap에 저장
    }
}
```



# String

## String, StringBuffer, StringBuilder

위의 3가지 클래스를 언제 사용하는지 정리하면 다음과 같다.

**String**          : 문자열 연산이 적고 멀티쓰레드 환경일 경우

**StringBuffer**   : 문자열 연산이 많고 멀티쓰레드 환경일 경우

**StringBuilder**  : 문자열 연산이 많고 단일쓰레드이거나 동기화를 고려하지 않아도 되는 경우



`String`은 불변(immutable)의 속성을 가진다. 때문에, String 클래스로 저장한 문자열에 문자열을 추가하면 새로운 String 인스턴스 생성하고, 처음에 선언했던 문자열은 GC에 의해 사라진다. 

반면 `StringBuffer`, `StringBuilder`는 가변(mutable) 속성을 가지기때문에 동일 객체 내에서 문자열을 변경하는 것이 가능하다.  `StringBuffer`는 동기화를 지원하여 멀티쓰레드 환경에서 안전하다(thread-safe). String도 불변성을 갖기때문에 thread-safe하다. 반면 `StringBuilder`는 멀티쓰레드 환경에서 사용하는것은 적합하지 않다. 하지만 단일쓰레드에서의 성능은 StringBuffer보다 뛰어나다.



# Class

## 하나의 자바 파일에 여러 개의 클래스 정의

a.java라는 파일에 public class로 만들수 있는 클래스는 파일명과 동일한 `class a {}` 뿐이다. 이외의 클래스를 파일 안에 생성시, 컴파일하면 똑같이 메서드 영역에 내용이 올라간다. 단, public 형태로 클래스를 선언할 수 없다.



## Inner Class





## this

`this`는 객체의 주소를 가리킨다. 메소드 호출 시, 실제로 선언된 메소드의 파라미터에 `this`라는 parameter가 추가된다. `this`는 메소드 내에서 첫번째 줄에서만 사용이 가능하고, 한 번만 사용할 수 있다.



### 용법

this의 용법은 크게 4가지다.

1. this() : 기본 생성자
2. this(variable) : 다른 생성자
3. this.variable : 자기 클래스의 변수 지칭
4. this.method() : 자기 클래스의 메서드 지칭



`super`를 이용하면 부모의 생성자/변수/메서드를 사용할 수 있다.



# 직렬화

## 직렬화(Serialize)란

객체를 한 줄로 늘어선 바이트 형태로 만드는 것으로, 전송이 가능해진다.

- 자바 시스템 내부에서 사용되는 객체나 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 **byte 형태**로 데이터를 변환하는 기술.
- JVM(Java Virtual Machine 이하 JVM)의 메모리에 상주(힙 또는 스택)되어 있는 객체 데이터를 바이트 형태로 변환하는 기술
- 파일 또는 네트워크를 통해 송수신(스트림)이 가능하게 만들어주는 기술



### 사용방법

1. 클래스 생성 시, `Seriazliable` 인터페이스를 implements한다.

   ```java
   public class User implements Serializable {//객체직렬화
       // 구현해야할 메서드가 없다.
   } 
   ```

2. `ObjectOutputStream` 클래스를 이용해 직렬화를 진행한다.

   ObjectOutputStream 클래스는 객체들을 출력하는 기능을 제공해 주고,출력 스트림에 출력하기 전에 직렬화를 수행한다. 

   Serializable 인터페이스를 앞에서 Implements 안해주면, NotSeriazliableException 발생

   ```java
   public static void main(String[] args) {
       User ob1=new User("민들레",20,89.5);
       User ob2=new User("개나리",22,73.5);
   
       //IO Stream(직렬화)
       try {
           ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("storage/user.txt")); // 파일로 저장
           oos.writeObject(ob1);
           oos.writeObject(ob2);
   
           System.out.println("저장 되었습니다");
           oos.close();
       } catch (FileNotFoundException e) {
           e.printStackTrace();
       } catch (IOException e) {
           e.printStackTrace();
       }
   }
   
   ```





## 역직렬화(Deserialize)

- byte로 변환된 Data를 원래대로 객체나 데이터로 변환하는 기술을 역직렬화(Deserialize)라고 부릅니다.
- 직렬화된 바이트 형태의 데이터를 객체로 변환해서 JVM으로 상주시키는 형태.



### 사용방법

```java
//IO Stream(역직렬화)
try {
    ObjectInputStream oos= new ObjectInputStream(new FileInputStream("storage/user.txt"));
    User ob1=(User)oos.readObject();
    User ob2=(User)oos.readObject();

    ob1.disp();
    ob2.disp();

    oos.close();

} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}

```

[참고한 블로그](https://nesoy.github.io/articles/2018-04/Java-Serialize)



# Lambda

람다식은 간단히 말해 함수를 변수처럼 사용할수 있는 개념이다.

자바람다식은 사실 완벽한 함수형 프로그래밍방식이라고 할 수는 없다. 자바 람다식은 함수형에 대해서 새로 정의한 것이 아니고, 기존에 존재하는 Interface의 형태를 빌어 람다식을 표현하기 때문이다. 그러므로 함수형 프로그래밍의 장점을 완전히 가지지는 못한다. 



## 형식

- ( parameters ) -> { expression body }
- ( parameters ) -> expression body
- () -> { expression body }
- () -> expression body    



## 사용

람다는 함수가 1개만 있는 인터페이스를 구현할때만 사용가능하다. (`@FunctionalInterface`)

클래스 생성 부분의 클래스명을 생략하고, 메소드명을 생략한다. 자료형을 알 수 있다면 자료형을 생략하고, return을 `->`로 표현한다.

### calc

> `@FunctionalInterface`를 사용하면, 인터페이스에 메소드 1개만 있는데 구현할 때마다 다른 식으로 사용할 수 있다.


```java
@FunctionalInterface
interface Calc{
	public double calc(int a, int b);
}

public class LambdaEx3 {
	public static void main(String[] args) {
		// Calc ob = (int a, int b) -> return a+b;
		Calc ob1 = (a, b) -> a+b;
		Calc ob2 = (a, b) -> a-b;
		Calc ob3 = (a, b) -> a*b;
		Calc ob4 = (a, b) -> a/b;
			
		System.out.println(ob1.calc(2,5));
		System.out.println(ob2.calc(2,5));
		System.out.println(ob3.calc(2,5));
		System.out.println(ob4.calc(2,5));
	}
}
```



### sort

sort할 때 `Comparator`라는 인터페이스를 구현해야한다. 이 부분을 람다식을 이용해 처리했다.

```java
public static void main(String[] args) {
    List<Student> list = new ArrayList<>();
    list.add(new Student("kim", 100));
    list.add(new Student("lee", 70));
    list.add(new Student("park", 85));
    list.add(new Student("hong", 60));
    list.add(new Student("kang", 95));

    System.out.println("*** 정렬 전 *** ");
    // void java.lang.Iterable.forEach(Consumer<? super String> action)
    list.forEach((m) -> System.out.println(m.getName() + " " + m.getTot()));

    // 정렬 2가지 방법
    System.out.println("*** 이름 오름차순 정렬 후 *** ");
    // 1. Collections
    Collections.sort(list, new Comparator<Student>() {
        @Override
        public int compare(Student o1, Student o2) {
            return o1.getName().compareTo(o2.getName());
        }
    });
    list.forEach((m) -> System.out.println(m.getName() + " " + m.getTot()));

    // 2. list.sort -> String
    System.out.println("*** 이름 내림차순 정렬 후 *** ");
    list.sort((m1,m2)->m2.getName().compareTo(m1.getName()));
    list.forEach((m) -> System.out.println(m.getName() + " " + m.getTot()));

    // 3. list.sort -> int
    System.out.println("*** 점수 오름차순 정렬 후 *** ");
    list.sort((m1,m2)->m1.getTot() - m2.getTot());
    list.forEach((m) -> System.out.println(m.getName() + " " + m.getTot()));
}
```



# Stream

컬렉션의 객체에 사용되며, 객체를 stream으로  얻어온 후 원하는 연산을 간략하게 구현하기 위한 API이다. 컬렉션의 객체를 일반 stream으로 리턴받는다.  

```java
Arrays.asList(1,2,3,4,5,6).stream();
```

또다른 컬렉션의 객체 및 메소드를 통해 추출 후 출력 할 수 있다.



## 주요 API

- forEach :stream의 요소를 순회해야 한다면 forEach를 활용할 수 있음

- map : stream의 개별 요소마다 연산을 할 수 있음

- limit : stream의 최초 요소부터 선언한 인덱스까지의 요소를 추출해 새로운 stream을 만듬

- skip : stream의 최초 요소로부터 선언한 인덱스까지의 요소를 제외하고 새로운 stream을 만듬

- filter : stream의 요소마다 비교를 하고 비교문을 만족하는 요소로만 구성된 stream을 반환

- flatMap : stream의 내부에 있는 객체들을 연결한 stream을 반환

- reduce : stream을 단일 요소로 반환

- collect : 각각의 메소드로 콜렉션 객체를 만들어서 반환

```java
// forEach
Arrays.asList(1,2,3,4,5,6).stream().forEach(System.out::println); //:: 스코프연산자

// map
Arrays.asList(1,2,3,4,5,6).stream()
.map(i -> i*i)
.forEach(System.out::println);

// limit
Arrays.asList(3,4,2,1,6,5).stream()
.limit(2) // maxSize
.forEach(System.out::println);

// skip
Arrays.asList(3,4,2,1,6,5).stream()
.skip(2)
.forEach(System.out::println);

// filter
Arrays.asList(1,2,3,4,5,6).stream()
.filter(i->i%2==0)
.forEach(System.out::println);   // 2 4 6

// filter를 다른방식으로
Arrays.asList(1,2,3,4,5,6).stream()
.forEach((i)-> {
    if(i % 2 == 0) System.out.println(i);
    });   // 2 4 6

// flatMap
Arrays.asList(Arrays.asList(1,2,3,4),Arrays.asList(5,6,7,8),Arrays.asList(9,10)).stream()
.flatMap(i->i.stream())
.forEach(System.out::println);		

// reduce
System.out.println(Arrays.asList(1,-2,3,-1,5).stream()
.reduce((a,b)->a-b)    // 1-(-2)=3-3=0-(-1)=1-5=-4
.get());

// collect
Arrays.asList(1,2,3,4,5).stream()
.collect(Collectors.toList()) // Collection으로 바꿔준다
.forEach(System.out::println);
```





# Singleton Pattern



# Exception


