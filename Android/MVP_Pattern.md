# MVP : Model, View, Presenter



## 1. MVP란?
UI(View)와 비즈니스 로직(Model)을 분리하고, 서로 간에 상호작용을 다른 객체(Presenter)에 그 역할을 줌으로써 서로의 영향(의존성)을 최소화 하는 디자인 패턴 입니다.
## 2. MVP의 구조

### 2.1. Model

	* 프로그램 내부적으로 쓰이는 데이터 저장 및 처리
	* View 와 Presenter 등 다른 어떤 요소에도 의존적이지 않은 독립적인 영역

### 2.2 View

	* UI를 담당하며 Activity, Fragment 등이 있다
	* Model에서 처리된 데이터를 Presenter를 통해 받아서 유저에게 보여줌
	* 유저 액션 및 라이프사이클 상태 변경을 주시하며 Presenter에 보내는 역할
	* Presenter을 이용해 데이터를 주고 받기 때문에 Presenter에 매우 의존적

### 2.3 Presenter

	* Model과 View 사이의 매개체
	* 뷰에게표시할 내용(Data)만 전달하며 어떻게 보여줄지는 View가 담당

![Alt text](https://media.vlpt.us/images/jojo_devstory/post/41896f61-0b9a-42a5-9534-06985e5452d4/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-03-10%20%EC%98%A4%ED%9B%84%202.38.41.png )

## 3. MVP의 기본 원칙들
	* View의 역할은 오직 Presenter가 명령한대로 화면을 그리는 것
	* View는 모든 유저와의 상호작용을 Presenter에 위임한다
	* View는 절대 Model을 직접 조작하지 않는다
	* Presenter는 View의 요구사항들을 Model에 전달하고 특정 이벤트가 일어날 경우 View를 제어한다
	* Model은 데이터를 서버, 데이터베이스, 파일 등으로부터 가져오는 역할을 한다.

### 3.1. 최상의 시나리오 
View 레벨 코드에는 if 문이 하나도 존재하지않는 것, Presenter에는 Android API를 전혀 사용하지 않는 로직만 있는 것

## 4. MVP의 장점
가장 좋은 장점은 Model과 View간의 결합도가 낮아지는 것입니다.
좋은 프로그램은 모듈간 결합도는 약하게, 응집력은 강하게 설계를 해야하는데 MVP 패턴은 결합도를 낮춰줍니다. 이렇게 결합도가 낮아지게 되면 새로운 기능을 추가하거나 기존에 있던 기능을 변경해야 할 때 관련된 코드만 수정하면 되기 때문에 확정성이 좋아집니다. 또한 테스트 코드를 작성하기 편리해지기 때문에 쉽고 안전한 코딩이 가능해 집니다.

## 5. MVP의 단점
가장 큰 단점은 애플리케이션이 복잡해질수록 View와 Presenter 사이의 의존성이 강해지는 단점이 있습니다. 그리고 시간이 지남에 따라 Presenter에 추가 비즈니스 로직이 집중되는 경향이 있습니다. 따라서 어느순간 거대해지고 분리하기도 어려운 Presenter가 탄생할 수 있습니다.

## 6. 참고

[안드로이드 아키텍처 패턴 - MVP란?](https://velog.io/@jojo_devstory/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-MVP%EA%B0%80-%EB%AD%98%EA%B9%8C)

[안드로이드에서 MVP 하는법](https://medium.com/@itsnothingg/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%97%90%EC%84%9C-mvp-%ED%95%98%EB%8A%94-%EB%B2%95-cc038a6abfc7)
