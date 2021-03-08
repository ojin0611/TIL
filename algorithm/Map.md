# Map

자바의 자료구조 Map은 key-value pair 형식을 갖고있다. 즉, key와 value는 매칭돼있다.

Map에서 특정 데이터를 찾을 때는 key를 이용해서 검색한다. 



## Method

`put(key,value), get(key), remove(key)`

`keySet()` : key들만 모아 Set으로 리턴

`values()` : value들만 모아 Collection으로 리턴



## Class

Map은 인터페이스로, Map을 구현한 클래스는 `HashMap, HashTable, TreeMap`등이 있다.

### HashMap

키를 저장할 때 해시 알고리즘(hash algorithm)을 사용해 검색속도가 매우 빠르다. `HashMap`은 키,값에 null을 저장할 수 있지만(1개) thread-safe하지 않다.

값이 추가로 들어오면 저장공간을 늘릴 때 과부하가 많이 발생한다. 그렇기에 초기에 저장할 데이터 개수를 알고있다면 Map의 초기용량을 지정해주는 것이 좋다.

> LinkedHashMap은 HashMap과 거의 같고, Map에 추가되는 순서를 유지한다.

```java
HashMap<Integer,String> map = new HashMap<Integer,String>(3);

map.put(1,"사과");
map.put(2,"바나나");
map.put(3,"포도");

map.get(2);

map.remove(3);
```

전체를 출력하려면 entrySet()이나 keySet() 메소드를 활용하여 Map의 객체를 반환받은 후 사용하는 것이 더 빠르다. key로 value를 찾는 과정에서 시간이 많이 소모되기때문이다.



### HashTable

`HashTable`은 키,값에 null을 저장할 수 없지만 thread-safe하다. HashMap 클래스에서 사용할 수 있는 메소드와 거의 같다. 



### TreeMap

`TreeMap` 은 key-value pair 데이터를 이진 검색 트리 형태로 저장한다. 같은 Tree구조인 `TreeSet`과 차이점은, TreeSet은 값만 저장한다면 TreeMap은 키와 값이 저장된 Map, entry를 저장한다는 점이다.

TreeMap에 객체를 저장하면 자동으로 정렬되는데, 키는 저장과 동시에 자동으로 오름차순으로 정렬된다. 데이터를 저장할 때 즉시 정렬하기때문에, 추가, 제거 시 HashMap보다 오래 걸린다. 하지만 정렬된 상태로 Map을 유지해야하거나, 정렬된 데이터를 조회해야한다면 더 효율적이다.

```java
TreeMap<Integer, String> map = new TreeMap<>(); // 생성
map.put(1,"사과");
map.put(1,"복숭아"); // 새로 입력되는 값으로 대치
map.put(2,"바나나"); 

map.remove(map.firstEntry()); // 최소 entry 출력 

map.clear(); // 모든값 제거

```

