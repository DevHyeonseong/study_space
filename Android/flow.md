#### cold stream
- 하나의 소비자에게 값을 보낸다
- 생성된 이후에 누군가가 소비(collect)하면 데이터를 발행

#### hot stream
- 하나 이상의 소비자(collector)에게 값을 보낼 수 있다
- 데이터 발행이 시작된 이후부터 모든 소비자에게 같은 데이터를 발행하고 구독자가 없는 경우에도 데이터를 발행한다

#### state flow
- hot stream, collect하는 순간 최신 값을 전달 받는다
- MutableStateFlow 업데이트를 담당하는 클래스가 생산자, StateFlow에서 collect하는 모든 클래스가 소비자
- ui를 업데이트해야 하는 경우, launchIn을 사용하면 안 됨. repeatOnLifecycle{}을 사용하여 업데이트해야함.
  - launchIn은 뷰가 표시되지 않는 경우에도 이벤트를 처리하기때문
