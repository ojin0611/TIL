# Process Synchronization

[toc]



공유 데이터의 동시 접근은 데이터의 불일치 문제를 발생시킬 수 있다. 일관성 유지를 위해 협력 프로세스간 실행 순서를 정해주는 메커니즘이 필요하다.



**Race condition**는 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황을 말한다. race condition을 막기 위해 병행 프로세스(concurrent process)는 동기화돼야한다.

운영체제에서 race condition이 발생하는 경우와 해결방법은 다음과 같다.

1. kernel 수행 중 interrupt 발생 -> interrupt 무시
2. kernel 수행중 context switch 발생 -> 프로세스가 kernel 모드 수행중일 땐, 다른 프로세스에게 CPU 주지 않는다.
3. multi processor에서 shared memory 내 커널 데이터 접근 발생 -> 커널 전체 또는 데이터에 lock



## Critical Section

공유 데이터에 접근하는 코드를 Critical Section이라고 한다. 하나의 프로세스가 critical section에 있을 때 다른 모든 프로세스는 critical section에 들어갈 수 없어야 한다. 이를 해결하기 위한 방법에는 3가지 충족 조건이 있다.

1. Mutual Exclusion (상호 배제) : 프로세스 하나가 critical section 수행중이면, 다른 모든 프로세스들은 그들의 critical section에 들어가면 안된다.
2. Progress (진행) : 아무도 critical section에 있지 않을 때 critical section에 들어가고자하는 프로세스가 있으면 들어가게 해줘야한다.
3. Bounded Waiting : 프로세스가 critical section 입장 요청을 한 후 지나치게 오래 기다리지 않게 해야한다. starvation 방지



## Peterson Algorithm

임계 영역 문제를 해결하기 위한 알고리즘 중 하나다.

1. 깃발 든다. (나 작업할래요)
2. 상대에게 턴을 준다. (일단 네 턴)
3. 아직 상대턴이거나, 상대가 깃발을 들었다면 기다린다.
4. 상대턴도 아니고, 상대 flag도 아닐 때 진행한다.



위의 경우에도 문제점은 발생한다. (ex. busy waiting)



## Mutex

뮤텍스는 단 1개의 프로세스나 쓰레드가 critical section에 access 가능하도록 해준다. 동기화 대상이 하나로, 리소스에 대한 접근을 조율하기 위해 lock을 사용한다. 



## Semaphore

Critical Section 문제를 소프트웨어가 아닌 하드웨어로 직접 접근하면 비교적 해결하기 쉽다. 세마포어는 추상 자료형으로, 멀티프로그래밍 환경에서 공유 자원에 대한 접근을 제한하는 방법이다. 동기화 대상이 하나 이상이고, P 연산과 V 연산이 있다. P,V연산과 critical section은 atomic하게 진행된다.

P : 임계구역 들어가기 전 수행. 

```c
while(S<=0) do waiting;
S--;
```



V : 임계구역에서 나올 때 수행.

```c
S++;
```



근데 위와 같은 방식일 때에도 문제는 발생하는데, 의미없이 while문이 실행되고있다는 점이다. 이를 보완하기 위해 **block & wakeup** 방식을 사용한다. 프로세스가 자원을 얻지 못하면 blocked 상태가 되고, 자원을 얻을 수 있을 때 프로세스를 깨우는 것이다.



## Problems of Synchronization

세마포어를 이용해도 동기화 과정에서 문제가 발생할 수 있다. 대표적인 문제는 3가지로 다음과 같다.

1. Bounded-Buffer Problem (Producer-Consumer Problem)
2. Readers and Writers Problem
3. Dining-Philosophers Problem



## Monitor

세마포어도 문제점이 있다. 코딩하기 힘들고, 정확성의 입증이 어렵고, 자발적 협력이 필요하며 한 번의 실수가 모든 시스템에 치명적인 영향을 미친다.

모니터는 동시 수행중인 프로세스 사이에서 abstract data type의 안전한 공유를 보장하기 위한 high-level synchronization construct다. 

작동 방식

1. 모니터 내부에 공유 변수에 대해 선언한다.
2. 공유 데이터에 접근하는 코드를 모니터 안에 정의한다.
3. 공유 데이터에 접근하기 위해서는 모니터 내부함수들을 이용해야한다.
4. 프로시져를 통해 공유데이터에 접근이 가능하도록 설계했다. 프로시져 안에 lock, unlock 구현.
5. 모니터 내에서는 한 번에 하나의 프로세스만 활동이 가능하다.
6. wait, signal을 이용한다.





# Deadlock

프로세스가 자원을 얻지 못해 다음 처리를 하지 못하는 상태다. 교착상태라고도 부르며, 한정된 자원을 여러 곳에서 사용하려고 할 때 발생한다. 



## Deadlock 발생 조건

1. Mutual Exclusion : 1개의 자원을 가질 수 있는 프로세스는 1개다.
2. No preemption : 프로세스는 자원을 강제로 빼앗기지 않는다.
3. Hold and wait : 자원을 가진 프로세스가 다른 자원을 기다릴 때, 기존에 갖고있는 자원을 holding함.
4. Circular wait : 자원을 기다리는 프로세스들간의 싸이클이 형성돼야한다.



## Deadlock 처리 방법

- Deadlock Prevention : 자원 할당 시 4가지 필요조건 중 어느 하나가 만족되지 않도록 하는것
- Deadlock Avoidance : deadlock 가능성이 없는 경우(safe state)에만 자원을 할당. 
- Deadlock Detection and recovery : deadlock 발생 허용하되, detection을 통해 recover
- Deadlock Ignorance : deadlock 무시하기. 대부분의 OS가 채택한다.



### Prevention

4가지 발생조건 중 하나를 막으면 된다.

1. 자원을 공유할 수 없게 만들면 된다.
2. 프로세스가 자원을 스스로 반납하도록 한다.
3. 프로세스가 자원을 기다려야할 때 이미 보유한 자원이 선점된다. 모든 자원을 얻을 수 있을 때 그 프로세스가 다시 시작된다. CPU, memory 등 state를 쉽게 저장하고 복구할 수 있는 자원에서 주로 사용된다.
4. 자원 유형에 할당 순서를 정한다.



### Avoidance

평생에 쓸 자원을 알고있다고 가정한다.

프로세스의 sequence(프로세스 실행 순서)가 safe하려면 i번째 프로세스의 자원 요청이 "가용 자원 + j번째 프로세스(j<i)의 보유자원"에 의해 충족돼야한다.

조건을 만족하면 다음 방법으로 모든 프로세스의 수행을 보장한다.

1. Pi의 자원요청이 즉시 충족될 수 없으면 Pj가 종료될때까지 기다린다.
2. Pi-1이 종료되면 Pi의 자원요청을 만족시켜 수행한다.



Avoidance 알고리즘으로는 Resource Allocation Graph Algorithm과 Banker's Algorithm이 존재한다.



### Detection and Recovery

**Detection**

Detection을 할 때 자원 1개를 사용하려는 프로세스(인스턴스)의 개수가 1개면 그래프를 이용하고, 2개 이상이면 (Banker's Algorithm과 유사하게) 테이블을 이용한다. 

1. Graph
   - Resource-Allocation Graph에서 자원을 빼버리고 프로세스만 남기면 Corresponding wait-for graph가 된다. 여기서 cycle이 존재하면 deadlock이 발생할 수 있다. 
2. Table
   - 각 프로세스마다 모든 자원에 대한 Allocation, Request, Available을 계산한다.
   - Request가 없으면 곧 반납한다고 가정한다.
   - 반납하면 Allocation이 Available에 추가되고, Available로 Request를 커버할 수 있으면 시퀀스가 존재하는 것이다.

**Recovery**

1. Process Termination
   - Deadlock 상태의 프로세스를 모두 abort한다. 또는 deadlock cycle이 제거될때까지 프로세스를 하나씩 abort한다.
2. Resource Preemption
   - 문제의 프로세스에게서 자원을 빼앗고 safe state를 만들어본다.
   - 자원을 뺏는 패턴을 바꿔줘야 starvation이 발생하지 않는다. 



### Ignorance

실제로 deadlock은 드물게 발생하므로 deadlock에 대한 조치 자체가 더 큰 overhead일 수 있기때문에 대부분의 범용 OS는 deadlock ignorance를 채택한다. 