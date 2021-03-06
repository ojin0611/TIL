[TOC]



## 순열

서로 다른 것들 중 몇 개를 뽑아 한 줄로 나열하는 것

BJ15649, BJ15654

### 재귀호출을 통한 순열 생성

{1,2,3}을 포함하는 모든 순열을 생성하는 함수

```
numbers[] // 순열 저장
isSelected[] // 인덱스에 해당숫자가 사용중인지 저장
perm(cnt) // cnt: 현재까지 뽑은 수의 개수
	if cnt == 3:
		순열 생성 완료
	else:
		for i from 1 to 3
			if isSelected[i] == true 
				then continue
			numbers[cnt] <- i
			isSelected[i] <- true
			perm(cnt+1)
			isSelected[i] <- false
		end for
```



**DFS**로 구현한다. 결과값은 numbers에 순서대로 값을 하나씩 추가한다.

재귀함수이므로, 종료 조건을 설정해야한다. 이 문제에선 숫자를 cnt개 선택한 상태에서 함수를 실행한다. 지금까지 선택한 숫자의 개수가 목표 개수(3)와 같을 경우, numbers에 있는 숫자를 출력해준다.



perm(cnt) 을 시작하면, 숫자 중 1개를 선택(i)하면서 시작한다.

각 숫자를 이미 선택했는지 확인하기 위해 isSelected라는 배열을 생성했다. 만약 **방금** 선택한 숫자(i)가 이미 선택한 숫자중에 있다면(isSelected[i]==true) continue를 통해 다음 루프를 돈다.

i가 선택한 적 없는 숫자라면, 방금 선택한 숫자(i)를 numbers의 **현재 위치(cnt)**에 추가한다.  숫자를 선택했으니 isSelected 내의 해당 숫자(i)를 true로 변경해준다.

여기부터 중요한데, DFS를 통해 깊게 들어갔다가 탐색을 마친 후, 거꾸로 돌아오는 코드를 작성할 것이기때문에 돌아올때는 방금 값을 true로 변경한 숫자(i)의 선택여부를 false로 바꾸면서 다음 루프를 돌아야한다. 

숫자를 cnt개 선택한 후, 다음 숫자를 탐색하기 위해 perm(cnt+1)을 실행한다. 탐색을 마친 후 방금 선택한 숫자(i)의 선택여부를 false로 바꿔준다. 탐색하다가 cnt==3인 경우엔 출력도 했을 것이다.

코드가 직관적이진 않을수도 있으니 로직 자체를 외워버리고 익숙해지는 것도 방법이다.



### 비트마스킹

기존에는 해당 숫자가 선택됐는지 여부를 저장하는 `isSelected`라는 배열을 생성했다. 이 문제에선 `flag`라는 int형 변수가 배열을 대신한다. flag를 2진수로 나타냈을 때, 각 비트가 T/F 역할을 해준다. int형이므로 32개의 비트까지 표현이 가능하다.

```
nPn -> N개 원소로 만들 수 있는 모든 순열
input[] : 숫자 배열
numbers[] : 순열 저장 배열

perm(cnt, flag) 
// cnt : 현재까지 뽑은 순열 원소 개수, flag : 선택된 원소에 대한 비트정보를 표현하는 정수
	if cnt == N
		순열 생성 완료
	else
		for i from 0 to N-1
			if (flag & 1<<i) != 0 then continue
			numbers[cnt] <- input[i]
			perm(cnt+1, flag | 1<<i)
		end for
end perm()
```

만약 `flag = flag | 1<< i` 를 한 뒤에 `perm` 에 `flag`를 넣으면 `perm` 메소드가 끝난 뒤에 flag값을 원상복귀 시켜야하기때문에 `perm` 안에 `flag | 1<<i` 를 넣어준다. 



### NextPermutation

현 순열에서 사전순으로 다음 순열 생성

ex) 3,2,5,4,1

1. 뒤쪽부터 탐색하며 꼭대기(i) 찾기. 교환위치=i-1

   > 뒤쪽 = 1, 꼭대기 i=3(값:5), 교환 위치(i-1)=2(값:2)

2. 뒤쪽부터 탐색하며 교환위치(i-1)와 교환할 큰 값 위치(j) 찾기

   > 뒤쪽 = 1, 2보다 큰 값(4)의 위치 j = 4

3. 두 위치 값(i-1, j) 교환

   > 3,4,5,2,1

4. 꼭대기위치(i=3)부터 맨 뒤까지 오름차순 정렬

   > 3,4,1,2,5

꼭대기가 첫번째 값이라면 순열 모두 완성한 것.  = 종료 조건!

```java
static boolean np() {
    // Step 1. 꼭대기 찾기. 그 앞 값을 바꾸려고
    int i = N-1;
    while(i>0 && input[i-1] >=input[i]) i--;

    if (i==0) return false;

    // Step 2. 교환값 찾기. Step 1보다 더 큰 값
    int j = N-1;
    while(j>i-1 && input[j] < input[i-1]) j--;

    // Step 3. 교환
    swap(i-1, j);

    // Step 4. 오름차순 정렬
    int k = N-1;
    while(k>i-1) {
        swap(k,i);
        k--; i++;
    }
    return true;
}
```





## 조합

서로 다른 n개의 원소중 r개를 순서없이 골라낸 것

nCr = (n-c)C(r-1) + (n-1)Cr

BJ 15650, BJ15655

### 재귀호출을 통한 순열 생성



```
input[] : n개의 원소를 갖고있는 배열
numbers[] : r개의 크기의 배열, 조합이 저장될 배열
comb(cnt, start) // cnt:현재까지 뽑은 조합원소 개수, start: 조합 시도할 원소의 시작인덱스
	if cnt==r
		조합 생성 완료
	else
		for i from start to n-1
			numbers[cnt] <- input[i]
			comb(cnt+1, i+1)
		end for
end comb()
```

DFS로 구현하는 것은 순열과 마찬가지다. 다만, 조합은 순서를 고려하기때문에 순열의 결과 중 순서에 맞는 결과만 선택한다. 그 방법은 **탐색의 범위를 이전 숫자보다 큰 숫자들로 제한**하는 것이다.

더 깊은 탐색 단계로 들어갈때마다, 현재 내가 결과에 저장한 숫자의 개수와 숫자의 값을 넘겨준다. 위의 코드에선 숫자의 값이 i고, 만약 숫자가 1~n이 아닌 임의의 숫자들이라면 해당 숫자의 index를 넘겨준다.

더 깊은 탐색에서는 탐색의 범위가 이전에 넣은 숫자(i)보다 더 큰 숫자(i+1)부터 탐색하기 위해 parameter start에 i+1을 넘겨준다. 



### NextPermutation 응용

N개 중 M개를 선택하는 조합이라면 0,0,...,0,0,1,1,...,1,1 형태로 0을 N-M개, 1을 M개 나열한다. 이 숫자들에 NextPermutation을 적용하고, input에서 1에 해당하는 숫자들만 선택하면 조합이 된다.



## 부분집합

입력된 숫자로 구성된 집합의 모든 부분집합(Power Set) 생성

재귀적으로 구현한다.

N개의 숫자들을 선택했는지/안했는지에 따라 2^N개의 부분집합이 생성된다.

그러면 N개를 지나가면서 포함 또는 비포함하면 하나의 부분집합이 완성된다.

### Idea

```
isSelected[] : 작성중인 부분집합에 포함/비포함 여부 저장한 배열. 엄밀히 말하자면 해당 숫자를 지나갔는지(처리했는지) 점검한다. cnt가 인덱스역할을 한다.

generateSubSet(cnt) // cnt : 현재까지 '처리'한 원소개수
	if (cnt==N) 
		부분집합 완성.
	else
		isSelected[cnt] <- true // cnt번째 숫자 포함
		generateSubSet(cnt+1) // cnt까지는 처리했고, cnt+1번째 숫자를 처리하러가자

		isSelected[cnt] <- false // cnt번째 숫자 비포함
		generateSubSet(cnt+1) // cnt까지는 처리했고, cnt+1번째 숫자를 처리하러가자
end generateSubSet()
```



아래 자바 코드(의 일부)도 위의 개념을 적용한 코드다.

```java
public static void select(int index, int taste, int calory) 
{
    // 조건을 충족하는 부분집합이라면
    if (calory<=L) 
	    // 연산하고싶은 부분
        taste_max = Math.max(taste_max, taste);	
    // 부분집합이 완성(index끝까지 도달)되거나, 더이상 부분집합에 추가할 수 없음
    if (index==N || calory>L) 
        return;

    // index + 1번째 음식을 처리하러 가자!
    // taste_calory에 저장돼있는 맛/칼로리를 하나씩 들어가면서 보는거야.
	
    // index번째 음식을 포함하고 다음을 처리하러 가자!
    select(index+1, taste+taste_calory[index][0], calory+taste_calory[index][1]);	
    // index번째 음식을 포함하지 말고 다음을 처리하러가자!
    select(index+1, taste, calory);									
}

```

