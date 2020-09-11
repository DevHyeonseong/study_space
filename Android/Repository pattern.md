## Repository pattern

### Repository pattern이란?
디자인 패턴 중 하나로, 데이터가 있는 여러 저장소(Local, Remote)를 추상화하여 중앙 집중 처리 방식을 구성하고, 데이터를 사용하는 로직을 분리시키기 위한 디자인 패턴 입니다. Repository가 추상화되어 있기 때문에 ViewModel은 언제나 같은 인터페이스로 데이터를 요청할 수 있다..
![다운로드](https://user-images.githubusercontent.com/54322066/92898923-57c4bf80-f459-11ea-9312-c508ee2ac84c.jpg)

### 왜 Repository pattern을 사용할까?
* 데이터 로직을 분리시킬 수 있다
* 중앙 집중 처리 방식이기 때문에 언제나 일관된 인터페이스로 데이터를 요청할 수 있다
* 클라이언트가 어떤 데이터를 사용할지 선택할 필요 없이, Repository에서 적절한 데이터를 제공한다
* 단위 테스트를 통해 검증이 가능하다
* 새로운 데이터 로직 코드를 쉽게 추가할 수 있다

### Repository pattern이 가지는 의의
* Repostiory는 데이터 저장소에 있는 데이터 객체를 캡슐화하고 더 객체지향적인 구조를 제공한다
* 모델과 비즈니스 로직을 깔끔하게 분리하며, ViewModel -> Model 간의 단방향 의존성 구현
* 수많은 ViewModel에서 호출하는 경우, 로직의 중복을 최소화 하는데 효과적

### 참고링크

[Repository Pattern?](https://0391kjy.tistory.com/39)
