# 코루틴 (coroutine)
- 서로 다른 두 서브루틴이 서로 협력하며 실행하는 동시적 프로그래밍
- 작업 수행 후 입출력과 같은 작업을 하게 되면 다른 코루틴에게 양보하며 서로 협력한다.
   - suspend와 resume의 반복.
   - 일반 함수와 다르게 여러 진입점이 존재
  
## 동시 프로그래밍
- 빠르게 여러 작업을 실행하여 마치 여러 작업이 '동시에' 실행되는 것처럼 보이는 프로그래밍
- 실제로 여러 작업이 동시에 실행되는 '병렬 프로그래밍'과 다르다.
- 코루틴은 한 스레드가 동시 프로그래밍을 하기에 병렬 프로그래밍보다 훨씬 적은 스레드로 많은 작업을 실행할 수 있다.
  - 그래서 가벼운 스레드라고 부르기도 한다.
 
## 코틀린 코루틴 
- 코루틴은 원래 존재하는 개념이고 코틀린은 코루틴 개념을 적용해서 비동기를 진행한다.
- 코루틴을 사용할 수 있는 빌더를 패키지를 통해 제공한다.

### coroutine scope
- 코루틴은 scope내에서 코루틴 빌더를 실행하여 관리한다.
- scope이 취소가 되면 내부 코루틴들이 전부 취소가 되기에 관리하기가 쉽다. (structured concurrency)
  - lifecycleScope, viewModelScope처럼 해당 객체의 생명주기가 종료되면 자동으로 코루틴을 취소하는 scope 존재
- coroutineName + Job + Dispatcher + CoroutineExceptionHandler로 구성되어 있다.

### Job
- 모든 코루틴은 Job을 리턴한다.
- coroutineScope은 이 Job을 통해서 내부 코루틴들을 관리한다.
  
### 코루틴 빌더
- launch
  - 코루틴 빌더
  - 리턴값이 없다.
- async / await
  - 코루틴 빌더
  - 리턴값이 존재
- runblocking
  - 코루틴 빌더
  - 코루틴이 끝날 때까지 호출한 스레드가 블락되기 때문에 주로 디버깅용으로 사용
 
### 코루틴 Dispatcher
- 코루틴을 실행할 스레드 풀을 선택할 수 있다.
- Main
  - 기본 스레드로 UI를 업데이트하고 상호작용한다.
- IO
  - 입출력 용 스레드
- Default
  - CPU 자원을 많이 사용하는 작업을 할 때 사용
- UnConfined
  - 특정 스레드에 속하지 않음. Main에서 실행하다가 다시 실행될 때는 IO에서 실행될 수도 있음


### 동기 vs 비동기
- 동기 (synchronous)
  - 요청한 서브 루틴에 대한 제어권을 가지고 있음
- 비동기 (asynchronous)
  - 요청한 서브 루틴에 대해 제어권을 가지고 있지 않음
  - 콜백 형태로 종료가 되었음을 알림
  
## 코틀린 코루틴 실행과정
- suspend 키워드를 붙이면 해당 메서드는 비동기적으로 실행된다
- suspend는 내부적으로 completion이라는 인자가 추가되는 데 이 completion이 현재 어디까지 실행되었는 지 상태값과 그 때까지의 변수들을 가지고 있다.
- suspend 함수는 정지 가능한 위치에 대해서 label값을 통해 switch문으로 갈라지는 데
- 0으로 실행됨 -> 0번 메서드 실행 (1로 변경) -> 0번 메서드가 끝나고 다시 함수를 호출 -> 1번 메서드 실행 (2번으로 변경) -> ... 이렇게 반복한다. 
