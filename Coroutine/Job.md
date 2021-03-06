### Job

#### 잡(Job)
* 잡은 파이어-앤-포겟(fire and forget) 작업이다
* 한 번 시작된 작업은 예외가 발생하지 않는 한 대기하지 않는다
* 코루틴빌더 launch() 또는 Job() 팩토리 함수를 사용해 잡을 생성할 수 있다

#### 예외처리
* 잡 내부에서 발생하는 예외는 잡을 생성한 곳까지 전파된다.
* 잡이 완료되기를 기다리지 않아도 발생한다

#### 라이프사이클

* New(생성) : 존재하지만 아직 실행되지 않는 잡
  * 기본적으로 생성될때 잡은 자동으로 시작됨
  * 자동으로 시작되지 않게 하려면 CoroutineStart.Lazy 사용
* Active(활성) : 실행중인 잡. 일시 중단된 잡도 활성으로 간주
  * start(), join() 으로 시작가능
  * start()는 잡의 완료를 기다리지 않지만, join()은 잡의 완료를 기다린다. 따라서 join은 코루틴 또는 일시 중단 함수에서 호출해야한다
* Completed(완료됨) : 잡이 더 이상 실행되지 않는 경우
* Canceling(취소 중) : 실행과 취소사이의 중간 상태
* Cancelled(취소 됨) : 취소로 인해 실행이 완료된 잡. 완료로 간주될 수 있다

#### 잡의 현재 상태 확인

* isActive : 잡이 활성 상태인지 여부. 잡이 일시 중지인 경우도 true 반환
* isCompleted : 잡이 실행을 완료했는지 여부
* isCancelled : 잡 취소 여부. 취소가 요청되면 즉시 true가 된다

![스크린샷 2021-02-02 오후 3 01 08](https://user-images.githubusercontent.com/54322066/106558822-c4a33800-6567-11eb-9d1b-5faeb861c11b.png)
![스크린샷 2021-02-02 오후 3 02 14](https://user-images.githubusercontent.com/54322066/106558817-c2d97480-6567-11eb-8d35-c0ba26eb4e21.png)
* cancel()호출시 Active -> Canceling -> Cancelled 상태변화. 취소됨은 완료로 간주되는 것을 확인할 수 있다

* 잡의 상태는 앞으로만 이동할 수 있으며, 뒤로 되돌릴 수 없다.
* 잡의 최종상태는 Cancelled 또는 Completed 이다

---
### Deferred

#### 디퍼드(Deferred)
* 디퍼드는 결과를 갖는 비동기 작업을 수행하기 위해 잡을 확장한다
* async()를 사용하거나, CompletableDeferred의 생성자를 통해 생성할 수 있다

#### 예외처리
* 디퍼드는 처리되지 않은 예외가 발생해도 자동으로 전파하지 않는다.
* 결과를 반환하는 await()가 호출될 때 디퍼드 안에서 발생한 예외가 전파된다

---
### Reference book : [Learning Concurrency in Kotlin]
