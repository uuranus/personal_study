# Chaenl
- 코루틴 사이에 Queue를 통해서 메시지를 쌓아두고 처리하는 방식
- 채널 종류
  - Unlimited -> 채널 크기 무한. 메모리가 허용하는 한 무한히 메시지를 버퍼에 쌓아두기 때문에 주의가 필요
  - Buffered -> 초기 설정된 사이즈까지만 메시지를 채울 수 있음. 넘치면 suspend됨
  - Rendezvous -> 채널 크기가 하나. 데이터가 존재하면 send가 suspend되고 비어있으면 receive가 suspend됨
  - Conflated -> 채널 크기가 하나인데 중복해서 send하면 값이 계속 덮어씌워짐
- BlockingQueue와 비슷하지만 스레드를 블락하지 않고 suspend한다는 차이가 있다.

# Flow
- 비동기적으로 연속적으로 데이터를 처리하는 코루틴
- producer, consumer 구조
- cold stream으로 consumer가 생길 때까지는 값을 생산하지 않는다.
  - 반대로 consumer가 없어도 계속 값을 생산하는 걸 hot stream이라고 함
  - Cold Stream: "API 호출이나 데이터베이스 쿼리와 같이 사용자가 요청해야 값이 생성되는 방식."
  - Hot Stream: "이벤트 버스나 센서 데이터와 같이 소비자가 없어도 데이터를 생산하는 방식."

### SharedFlow, StateFlow
- hot stream Flow
- sharedFlow는 여러 consumer가 하나의 flow를 구독할 수 있음
  - 그러나 collect할 때마다 새로 값을 생산하던 cold stream과 달리 기존 캐시 값을 사용해서 반복 사용
- stateFlow
  - State에 특화된 Flow
  - null값이 존재할 수 없는 State 특성에 맞춰 초기값이 항상 존재하고
  - 1개의 값을 캐싱해서 새로운 consumer가 collect할 때마다 해당 값을 반환
  - StateFlow는 상태에 특화되어 항상 마지막 값을 캐싱하고, null 값을 허용하지 않는다.
  - stateIn 메서드를 통해 일반 flow는 stateflow로 변환할 수 있음
    
  ``` kotlin
    val stateFlow = flow.stateIn(
      scope = viewModelScope,
      started = SharingStarted.WhileSubscribed(5000),
      initialValue = 0
    )
  ```
  
  - 이 때, started는 값 sharing이 시작되고 끝날 때까지의 전략인데 이 중 WhileSubscribed를 자주 사용
  - WhileSubscribed(stopTimeoutMillis)
    - producer가 더 이상 consumer가 존재하지 않아도 stopTimeoutMillis만큼은 살아있게 하는 것이다
    - 그 이유는 configuration change같이 빠른 시간에 다시 뷰가 그려지는 경우 멈췄다 다시 생성하는 것보다 기존 stream을 재사용하는 게 더 효율적이기 때문
 
### repeatOnLifecyle
- consumer도 코루틴 scope에서 collect해야 한다.
- LifecycleScope 주로 사용하며 이 안에서 repeatOnLifecycle을 사용한다.
  - why? 그냥 collect를 하면 onStop을 통해 앱이 백그라운드로 이동해 더 이상 소비하지 않아도 upstream이 멈추지 않는다.
  - 이는 비효율적이기에 collect가 suspend가 되면 upstream도 멈추었다가 onStart가 되면 다시 시작되도록 하는 메서드가 repeatOnLifecycle이다.
