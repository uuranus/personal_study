# Operation System (OS)
하드웨어와 소프트웨어 자원을 관리하고 프로그램이 안전하고 효율적으로 동작하도록 하는 소프트웨어

운영체제도 소프트웨어이기에 RAM에 올라가 있고 메모리 중 커널 영역에 존재한다. (다른 응용 소프트웨어는 사용자 영역에 존재)

## 커널 (kernel)
- 프로그램은 컴퓨터 하드웨어 자원을 사용하고 싶으면 운영체제에게 요청해야 함. 일종의 하드웨어 문지기 역할
- 운영체제가 제공하는 서비스를 얻을 수 있는 모드(커널 모드)와 사용할 수 없는 모드(사용자 모드)로 구분
- 프로그램은 사용자 모드로 실행되다가 시스템 콜 (system call)을 통해서 커널 모드로 변경되어 운영체제의 서비스를 사용하고 다시 사용자 모드로 돌아옴

# 운영체제가 하는 일

## 프로세스 관리
- 메모리에는 여러 개의 프로세스가 동시에 실행될 수 있으며 더 이상 사용되지 않는 프로세스는 메모리에서 삭제해야 함
- 운영체제는 PCB (Process Control Block)이라는 구조체를 통해서 프로세스들을 관리. 일종의 이름표 역할이며 메모리 커널 영역에 존재
  - PCB 는 PID, 프로세스 상태, 메모리, 스케줄링 정보 등을 포함하고 있음
- 연속 할당
  - 프로세스를 순서대로 연속적으로 할당
  - 중간에 프로세스가 종료되면 중간중간 빈공간이 발생
    - 최초 적합 (first fit), 최악 적합 (worst fit), 최적 적합 (best fit)으로 프로세스 할당
    - but 그래도 조금씩 빈공간이 발생 -> 외부 단편화 (external fragment)가 발생했다고 한다. 
- 불연속 할당
  - 프로세스와 메모리 공간을 일정한 크기로 잘라서 불연속으로 배정한 후 두 공간을 매핑해서 할당하는 방식
  - 프로세스는 프레임이라고 하고 메모리 공간은 페이지라고 한다. 그리고 페이지 테이블이 이 프레임과 페이지를 매핑한 정보를 가지고 있음
    - 페이지 테이블도 메모리에 로드되기에 TLB 캐시를 통해서 메모리 접근 속도를 높일 수 있음
  - 페이지 공간에 비해 프레임 크기가 작아서 공간이 비는 경우 발생 -> 내부 단편화 (internal fragment)가 발생했다고 한다.
- 모든 프로세스 데이터를 다 로드하지 않고 사용되고 있는 부분만 로드하여서 더 많은 프로세스를 실행할 수 있도록 하는 가상 메모리 방식도 존재

## CPU 스케줄링
- 프로세스가 CPU를 잘 나눠서 사용할 수 있도록 CPU 배정을 관리함

**비선점 스케줄링 (non-preemptive scheduling)**
  - 해당 프로세스가 다 종료될 때까지 CPU 자원을 다른 프로세스가 뺏을 수 없는 스케줄링
  - FCFS( First Come First Serve) -> 먼저 온 프로세스부터 실행. convoy effect 발생 가능성
  - SJF (Shortest Job First) -> 가장 실행시간이 짧은 프로세스부터. 응답 시간은 좋아지나 현실적으로 어느 프로세스가 몇 분 걸릴지는 미리 알 수 없음
  - Priority -> 우선순위가 높은 프로세스부터 실행. 기아 (starvation) 현상 발생 가능성. aging으로 보완 가능

**선점 스케줄링 (preemptive scheduling)**
  - 해당 프로세스가 다 종료되기 전에 CPU 자원을 다른 프로세스에게 양보할 수 있음
  - RR (Round Robin) - 일정 time slice 마다 프로세스를 변경.
  - SRT (Shortest Remaining Time first) - 실행시간이 제일 적게 남은 프로세스부터 실행.
  - Multilevel Queue - 우선순위별 큐(Queue)가 존재하고 높은 우선순위 큐부터 실행. 큐 간의 이동이 불가해 기아 현상 가능성
  - Multilevel Feedback Queue - 우선순위 큐 간 Job의 이동이 가능. aging을 통해 기아 현상을 막을 수 있음 

## 입출력장치 관리

**인터럽트 처리 과정**
  - 장치 드라이버는 요청한 작업을 완료하고 나면 인터럽트를 통해서 CPU에게 알림.
  - CPU는 해당 인터럽트와 같이 온 인터럽트 서비스 루틴으로 점프하여 명령어를 실행.
  - 이 때 기존에 작업하던 내용은 PCB에 저장하고 있다가 완료 후 다시 백업해서 이어서 실행.
  - 입출력을 비동기 인터럽트로 하는 이유
    - CPU가 계속 입출력이 완료되었는 지 확인하는 것은 비효율적이기 때문에 

**인터럽트 종류**

동기 인터럽트
  - CPU 내부에서 발생하는 인터럽트.
  - ex. exception, system call

비동기 인터럽트
  - 외부 인터럽트
  - ex. 입출력 완료, 하드웨어 이상

## 네트워킹
- 다른 호스트와 네트워크를 통해 정보를 주고받을 수 있음.

## 사용자 관리
- 한 컴퓨터에 여러 유저가 계정을 만들어서 사용 가능.