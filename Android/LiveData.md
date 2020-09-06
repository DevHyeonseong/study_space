# LiveData

## 1. LiveData란?
	* Data의 변경을 관찰 할 수 있는 Data Holder 클래스
	* Activity, Fragment등과 같은 Component의 LifeCycle을 알고 있다. 따라서 활성상태(active)일때만 데이터를 업데이트 한다.
	* Observer 객체와 같이 사용된다. 데이터에 변화가 일어날 경우 LiveData는 등록된 Observer 객체에 변화를 알려주고, Observer의 onChanged() 메서드가 실행된다.

### 1.1. LifeCycleOwner
	* Android LifeCycle을 알고 있는 클래스
	* getLifeCycle() 밖에 없는 단일 메서드 인터페이스 클래스
	* LiveData는 Observer 메서드의 변수로 사용함으로써 LifeCycle을 인식함

## 2. LiveData 장점
* Data와 UI간 동기화
* 메모리 누수(Memory Leak)가 없다
* Stop 상태의 Activity와 Crash가 발생하지 않는다
* 생명주기에 대한 추가적인  handling을 하지 않아도 된다
* 항상 최신 데이터를 유지한다
* 자원(Resource)를 공유할 수 있다

## 3. LiveData 사용시 알아야 할 점
* Generic을 사용해 관찰하고자 하는 데이터의 타입을 갖는 LiveData 인슽턴스를 생성한다
* LiveData 클래스의 observe() 메서드를 사용해 Observer 객체를 LiveData 객체에 "결합" 한다
* Observer 객체를 생성하고, 데이터 변화가 일어 났을 때 수행되는 onChanged() 메서드를 정의한다

## 4. 참고
[LiveData...넌 누구냐?](https://velog.io/@jojo_devstory/Android-LiveData...%EB%84%8C-%EB%88%84%EA%B5%AC%EB%83%90)
