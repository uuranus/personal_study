
# 교착상태 (Deadlock)
- 프로세스들이 서로 자원을 점유한 채 상대 프로세스의 자원을 기다리며 무한 대기 상태에 빠지는 현상
- ex. 식사하는 철학자 문제, 프로세스끼리 서로의 자원이 할당되기를 기다림

## 교착상태의 원인
1. 비선점 (non-preemptive)
   - 다른 프로세스가 사용하는 자원을 뺏어올 수 없음
2. 점유 후 대기 (hold and wait)
   - 프로세스가 자원을 점유한 채로 기다림 
3. 상호 배제 (mutual exclusion)
   - 한 번에 한 프로세스만 해당 자원에 접근할 수 있음
4. 원형 대기 (circular wait)
   - 자원 할당 그래프를 그렸을 때 사이클이 발생
   - but. 사이클이 있다고 무조건 교착상태가 일어나는 건 아님

**이 네 가지 조건이 모두 충족되어야** 교착상태가 발생할 가능성이 생긴다.
  
## 교착상태 해결 방법
1. 예방 (Prevention)
   - 4가지의 원인 중 한 가지가 일어나지 못하도록 하는 것 
   1. 비선점을 없애기 -> 모든 자원이 선점 가능한 건 아님 ex. 프린터기 출력하다가 다른 거 출력할 수 없음
   2. 점유 대기를 없애기 -> 프로세스가 필요한 모든 자원을 확보하지 못하면 지금까지 획득한 자원을 모두 반납하고 대기. 자원의 활용률이 낮아짐 & 자원 많이 쓰는 프로세스 기아 현상 가능성
   3. 상호 배제를 없애기 -> 마찬가지로 프린터기처럼 여러 프로세스가 사용하면 안 되는 자원이 많기에 현실적으로 불가능
   4. 원형 대기를 없애기 -> 자원마다 번호를 붙이고 오름차순으로 자원을 할당. but. 자원에 다 번호 붙이기도 어렵고 번호를 붙임으로서 자원의 사용률이 비효율적일 수 있음
2. 회피 (Avoidance)
    - 실제로 발생 가능한 자원 할당 상황을 미리 예측해 교착상태를 회피
    - 안전 상태 (교착 상태가 발생하지 않고 프로세스들에 안전하게 자원을 할당할 수 있는 상태)일 때만 자원을 할당하는 방식
      - 안전 순서열 (안전하게 프로세스에 자원을 할당할 수 있는 순서)이 존재하면 안전 상태 (safe state)라고 함 <-> 불안전 상태 (unsafe state)
      - Banker’s Algorithm이라고도 부름
3. 검출 후 회복 (Dectection and Recovery)
   - 교착 상태가 발생한 후 회복하는 것
   - 선점을 통한 회복 -> 사용 중인 자원을 강제로 회수해 다른 프로세스에 할당하고, 교착상태가 해결될 때까지 반복
   - 강제 종료를 통한 회복 -> 중요도가 낮은 프로세스부터 종료하는 방식으로 교착상태를 해결
4. 무시 (Ignoring)
   - 타조 알고리즘 (ostrich algorithm)이라고도 부르며
   - 현실적으로 교착상태의 발생 빈도가 적고, 이를 감지하고 회복하는 비용이 더 크다고 판단될 때 사용
