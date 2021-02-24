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

`HashMap`은 키,값에 null을 저장할 수 있지만(1개) thread-safe하지 않다.

키를 저장할 때 해시 알고리즘(hash algorithm)을 사용해 검색속도가 매우 빠르다.

> LinkedHashMap은 HashMap과 거의 같고, Map에 추가되는 순서를 유지한다.

### HashTable

`HashTable`은 키,값에 null을 저장할 수 없지만 thread-safe하다. HashMap 클래스에서 사용할 수 있는 메소드와 거의 같다. 



### TreeMap

`TreeMap` 은 key-value pair 데이터를 이진 검색 트리 형태로 저장한다. 데이터의 추가, 제거가 매우 빠르다. 순서가 존재한다.