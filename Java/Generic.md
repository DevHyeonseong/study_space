# 제네릭(Generic)

## 1. 제네릭(Generic)이란?
제네릭이 나온 배경은 자료형의 객체들을 다루는 메서드나 클래스에서 컴파일 시간에 자료형을 검사해 적당한 자료형을 선택할 수 있도록 하기 위해서입니다. 제네릭을 사용하면 객체의 자료형을 컴파일할때 체크하기 때문에 객체 자료형의 안정성을 높이고 형 변환의 번거로움이 줄어듭니다.

## 2. 제네릭 장점
* 타입 안정성을 제공한다
* 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다
* 원하지 않는 타입의 객체가 포함되는 것을 막을 수 있다

## 3. 제네릭 예제

### 3.1. 싱글 파라미터 제네릭 타입 클래스
```java
public class SingleGeneric<T>{
    private T t;

    public T get(){
        return t;
    }
	
    public void set(T t){
        this.t = t;
    }
}
```
### 3.2. 멀티 파라미터 제네릭 타입 클래스
```java
public class MultiGeneric implements Map.Entry<K,V>{
    private K key;
    
    private V value;

    public Entry(K key, V value){
        this.key = key;
        this.value = value;
    }

    public K getKey(){
        return this.key;
    }

    public V getValue(){
        return this.value;
    } 
}
```
### 3.3. 제네릭 메서드
```java
public class GenericMethod{
    public <T> T method(T item){
        // TODO
    }
}
```
## 4. 참고
[제네릭(Generic)문법 정리](https://cornswrold.tistory.com/180)
