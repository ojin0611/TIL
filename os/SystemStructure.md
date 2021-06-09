[toc]

# 컴퓨터 시스템 구조

![image-20210125202029011](images/image-20210125202029011.png)

## CPU

- CPU는 1개다. 누구한테 줄 지 CPU 스케줄링을 통해 결정한다.

- 매 클럭마다 메모리에서 instruction을 하나씩 읽고 실행한다.
- 매번 프로그램 카운터가 가리키고있는 곳의 명령을 수행한 뒤, 다음 명령을 수행하기 직전에 interrupt line이 세팅되었는지 체크한다.



## 메모리

- CPU가 잘 처리할 수 있게 준비하는 곳. 
- 한정된 메모리를 분배하는 것이 중요하다.



## 디스크

- 파일 관리. 파일을 어떻게 보관할지, 어떻게 파일을 빨리 읽을지 고민한다.



## Device Controller

- 키보드, 디스크 등 메모리가 아닌 곳에서 CPU에게 요청하기 위해 디바이스마다 device controller가 존재한다. 해당 device가 일을 다하면 CPU에게 interrupt를 날린다.
- device controller가 작업하는 공간, 즉 device마다 존재하는 별도의 작업공간이 local buffer다.



## Interrupt Line

- 각종 interrupt들이 기록돼있다. 
- CPU는 작업중인 instruction이 끝날때마다 interrupt line을 확인하고, interrupt가 있으면 OS에게 CPU 제어권을 넘긴다. 
- 이 때 mode bit 값이 변환된다. (0 : kernel mode, 1 : user mode)



## Timer

- 프로그램들에게 시간을 할당해준다. CPU가 독점되는것을 방지한다. 
- Timer도 Interrupt line에게 신호를 보낸다.



## DMA (Direct Memory Access)

- CPU에게 interrupt가 너무 많이 들어와 overhead가 크기때문에, 중간에서 interrupt를 한 번 실행해 메모리에 올려놓는다.
- local buffer에서 쌓인 작업량이 어느정도 채워지면 그 때 메모리로 보내고 interrupt를 날린다.
- 메인메모리에 DMA와 CPU 둘 다 접근할 수 있기 때문에, 메모리 컨트롤러는 CPU와 DMA 사이의 교통정리 역할을 한다.



# 