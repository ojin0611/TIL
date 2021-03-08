# Set

java에는 Set 인터페이스가 있다. 



# HashSet

HashSet은 Set 인터페이스의 구현 클래스다. 객체를 중복해서 저장할 수 없고, 하나의 null 값만 저장할 수 있다. 저장순서가 유지되지 않는다. (LinkedHashSet 클래스를 사용하면 저장순서가 유지된다.)

값을 추가하거나 삭제할 때, 내가 수정하고자하는 값이 Set 내부에 있는지 검색한 뒤 추가나 삭제를 해야하므로 속도가 List 구조에 비해 느리다.



## 중복을 걸러내는 과정

1. 객체의 hashCode() 메소드를 호출해서 해시코드를 얻어낸 다음, 객체들의 해시코드와 비교한다.
2. 같은 해시코드가 있다면 equals() 메소드로 두 객체를 비교한다
3. true가 나오면 중복저장을 하지 않는다.

문자열(String 클래스)의 경우 hashCode()와 equals() 메소드를 Override해서, 같은 문자열일 경우 hashCode()의 리턴값을 같게, equals()의 리턴값을 true로 나오도록 했다.



## 사용법

```java
HashSet<Integer> set = new HashSet<>(); // initial capacity(16), load factor(0.75)
HashSet<Integer> set2= new HashSet<Integer>(Arrays.asList(1,2,3));
```

값 추가는 add(e), 값 삭제는 remove(e), 모든 값 제거는 clear(), 크기는 size()를 이용한다. 값 검색은 contains(e)를 이용한다!



# TreeSet

HashSet과 마찬가지로 Set 인터페이스를 구현한 클래스지만 TreeSet은 이진탐색트리 구조로 이루어졌다. 추가와 삭제에 시간이 조금 더 걸리지만 정렬,검색에 높은 성능을 보인다. 기본적으로 nature ordering을 지원하며, 생성자의 매개변수로 Comparator 객체를 입력하여 정렬 방법을 임의로 지정해줄 수도 있다.



## 사용법

```java
TreeSet<Integer> set1 = new TreeSet<Integer>();
TreeSet<Integer> set2 = new TreeSet<Integer>(set1);
```

값 추가는 add(value). 값 추가에 성공하면 true, 내부에 값이 존재한다면 false를 반환한다.

값 삭제는 remove(value). 마찬가지로 삭제에 성공하면 true, 실패하면 false.

최소값은 `set.first()`, 최대값은 `set.last() `

`set.higher(value)`는 입력값보다 큰 데이터중 최솟값, `set.lower(value)`는 입력값보다 작은 데이터중 최대값을 리턴한다.