# CPU Scheduling

[TOC]

프로세스는 CPU Burst, I/O Burst가 번갈아가며 작업이 진행된다. I/O bound job(many short CPU bursts)의 경우, CPU를 계속 점유하고 있으면 다른 프로세스가 CPU를 얻지 못하는 경우가 발생하기때문에 CPU 스케줄링을 통해 CPU를 프로세스들에게 효율적으로 나누는 것이 필요하다.



## CPU Scheduler

CPU 스케줄러는 Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다. 

Dispatcher는 CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다. 이 과정을 context switch라고 한다.

CPU 스케줄러는 운영체제 안에 있는 코드를 부르는 말이고, Dispatcher는 운영체제 안에 있는 특정 기능으로, 별도의 하드웨어는 아니다. 



CPU 스케줄링은 2가지로 나뉜다.

**Nonpreemptive** : 비선점형. 한 번 프로세스에게 CPU를 주면 **빼앗기지 않는** 것으로, 작업하는동안 CPU를 보장해준다. 

**preemptive** : 선점형. 프로세스에게서 CPU를 강제로 빼앗을 수 있다.



## 성능

CPU 스케줄링 알고리즘의 효율을 판단할 수 있는 척도들은 다음과 같다.

> 시스템(식당, CEO)과 프로그램(소비자) 관점에서 생각하면 이해가 편하다.

시스템관점

**CPU utilization** : CPU를 놀게하지 않기. keep the CPU as busy as possible. 주방장이 쉬지않고 일하는 정도

**Throughout** : 처리량. CPU가 단위시간동안 완료하는 프로세스의 실행 개수. 방문한 손님 숫자

프로그램/프로세스 관점

**Turnaround time** : 소요시간, 반환시간. CPU를 쓰러 들어와서 다 쓰고 나갈때까지 걸린 시간. CPU burst에 쓴 시간. 손님이 음식 먹고간 시간

**Waiting time** : 줄서서 기다린 시간. 먹는동안 기다린 시간들의 합.

**Response time** : Ready queue에 들어와 처음으로 CPU를 얻기까지 걸린 시간. 현대 time sharing에서 중요한 개념이다. 단무지부터 받으면 짧아진다. 



# CPU Scheduling 알고리즘

## FCFS

선착순

## SJF

- shortest job first
- CPU burst가 제일 짧은 프로세스에게 먼저 CPU를 주는 방법
- Starvation 문제 발생할 수 있다. 
- Waiting time 예측이 어렵다.



## Priority Scheduling

- 우선순위가 높은 프로세스에게 CPU를 준다.
- Starvation 발생 가능. 우선순위 낮은 프로세스는 계속 실행되지 않는다.
- 위의 Starvation 해결하기 위해 aging 기법을 사용한다. 시간이 지나면 우선순위를 높여준다.



## Round Robin

- 각 프로세스는 동일한 시간의 할당 시간(time quantum)을 가진다.
- Q time unit만큼 할당시간을 갖는다.
- 응답시간(response time)이 빨라진다는 것이 RR의 장점이다.
- Q가 크면 FCFS와 유사해지고, Q가 작으면 context switch의 오버헤드가 커진다.
- 일반적으로는 긴 프로세스, 짧은 프로세스가 섞여있기때문에 Round robin이 적합한 알고리즘으로 선택되는 경우가 많다.



## Multi-level Queue

위의 알고리즘들은 프로세스들이 1개의 queue에 존재한다고 가정했을 때다. 실제로는 우선순위에 따라 여러 개의 queue가 존재한다. 

ready queue를 foreground/ background로 분류하고, 각 큐의 특징(interactive or no human interaction)에 따라 독립적인 스케줄링 알고리즘을 적용한다.

CPU time을 각 Queue마다 적절한 비율로 할당한다.





## Multi-level Feedback Queue

위의 multi-level queue는 큐가 정해지면 다른 queue로 이동하지 못한다. (ex. system processes, interactive process, batch processes, ...) 반면 Multi-level Feedback queue는 queue간 이동이 가능하다. 

각 queue마다 q-time이 다른데, 첫 번째 queue가 가장 q-time이 짧다. 우선 첫 queue로 프로세스가 들어오고, 작업시간이 짧으면 첫 큐에서 작업이 끝난다. 작업이 길어지면 점점 다음 queue(q-time이 길다)로 이동한다. 



