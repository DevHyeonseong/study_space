## Kotlin - scope function : let, also, apply, run, with, use

### 1. let()

```kotlin
public inline fun <T, R> T.let(block: (T) -> R): R = block(this)
```
함수를 호출하는 객체 T를 이어지는 block의 인자로 넘기고 block의 결괏값 R을 반환합니다.

```kotlin
fun main(){
    var a = 3
    println(a) // 출력 : 3
    a = a.let { it+2 }  // a = a+2 와 같다
    println(a) // 출력 : 5
}
```
a.let { it +2 }는 a를 { it + 2 }의 인자인 it로 넘겨서 그 결괏값인 5를 반환합니다.

---
### 2. also()
```kotlin
public inline fun <T> T.also(block: (T) -> Unit): T { block(this); return this }
```
함수를 호출하는 객체 T를 이어지는 block에 전달하고 객체 T 자체를 반환합니다. 
```kotlin
fun main() {
    var a = 3
    println(a) // 출력 : 3
    a = a.also { it + 2 }
    println(a) // 출력 : 3
    a.also { println(it + 2) } // 출력 5
}
```
a.also { it + 2 } 는 a를 { it +2 }의 인자인 it로 넘겨서 작업을 수행하되 호출 함수 a 자체를 반환합니다.

---
### 3. apply()

```kotlin
public inline fun <T> T.apply(block: T.() -> Unit): T { block(); return this }
```
함수를 호출하는 객체 T를 이어지는 block으로 전달하고 객체 자체인 this를 반환합니다.
```kotlin
fun main() {
    data class Person(var name: String, var skills: String)
    var person = Person("Kildong", "Java")
    println(person) // 출력 : Person(name=Kildong, skills=Java)

        person.apply {
            name = "Minsu"
            skills = "Swift"###
        }
    println(person) // 출력 : Person(name=Minsu, skills=Swift)

        person.also {
            it.name = "June"
            it.skills = "Kotlin"
        }
    println(person) // 출력 : Person(name=June, skills=Kotlin)
}
```
얼핏 보면 also()와 별 차이 없어보이지만 객체를 넘겨받는 방식이 다릅니다.
also() 함수는 it를 생략할 수 없지만 apply() 함수에서는 this를 생략할 수 있습니다.
따라서 apply() 함수는 특정 개체를 초기화하는데 아주 유용합니다.

---
### 4. run()

```kotlin
public inline fun <R> run(block: () -> R): R = return block()
public inline fun <T, R> T.run(block: T.() -> R): R = return block()
```
run() 함수는 인자가 없는 익명 함수처럼 동작하는 형태와 객체에서 호출하는 형태, 2가지로 사용할 수 있습니다.
객체 없이 run() 함수를 사용하면 인자 없는 익명 함수처럼 사용할 수 있습니다. 
```kotlin
fun main() {
    var skills = "Kotlin"
    println(skills) // 출력 : Kotlin

    val a = 10

    skills = run{
        val level = "Kotlin Level : " +a
        level // 마지막 표현식 반환
    }
    println(skills) // 출력 : Kotlin Level : 10
}
```
인자 없이 활용할 경우 block을 수행하고 마지막 문장을 반환합니다.
```kotlin
fun main() {
    data class Person(var name: String, var skills: String)

    var person = Person("Kildong", "Kotlin")

    val returnObj = person.apply {
        this.name = "Sean"
        skills = "Java" // this 생략 가능
        "Success" // 사용되지 않음
    }
    println(person) // 출력 : Person(name=Sean, skills=Java)
    println("returnObj: $returnObj") // 출력 : returnObj: Person(name=Sean, skills=Java)

    val returnObj2 = person.run{
        this.name = "Dooly"
        skills = "C#" //  this 생략 가능
        "Success"
    }
    println(person) // 출력 : Person(name=Dooly, skills=C#)
    println("returnObj2: $returnObj2") // 출력 : returnObj2: Success

    val returnObj3 = person.run{
        this.name = "Lubi"
        skills = "Swift" //  this 생략 가능
    }
    println(person) // 출력 : Person(name=Lubi, skills=Swift)
    println("returnObj3: $returnObj3") // returnObj3: kotlin.Unit
```
인자가 있을 경우 위 처럼 활용할 수 있습니다. 다만 apply() 함수는 마지막 표현식을 반환하지 않지만
run() 함수는 마지막 함수를 반환합니다.  물론 마지막 표현식을 구현하지 않으면 Unit이 반환됩니다.

---
### 5. with()
```kotlin
public inline fun <T, R> with(receiver: T, block: T.() -> R): R recevier.block()
```
with() 함수는 인자로 받는 객체를 이어지는 block의 receiver로 전달하며 결괏값을 반환합니다. 
with () 함수는 run() 함수와 기능이 거의 동일한데, run() 함수의 경우 receiver가 없지만 with() 함수에서는 receiver로 전달할 객체를 처리하므로 객체의 위치가 달라집니다.
```kotlin
fun main() {
    data class User(val name: String, var skills: String, var email: String? = null)

    val user = User("kildong", "default")

    val result = with(user){
        this.skills = "Kotlin"
        email  = "kildong@example.com" // this 생략가능
        // 아무것도 없으므로 Unit 반환 표현식을 넣는 다면 표현식이 반환됨
    }

    println(user) // 출력 : User(name=kildong, skills=Kotlin, email=kildong@example.com)
    println("result: $result") // 출력 : result: kotlin.Unit
}
```
user를 인자로 받아서 처리합니다. with() 함수도 run()과 마찬가지로 this를 생략할 수 있습니다.

---
### 6. use()
```kotlin
public inline fun <T: Closeable?, R> T.use(block: (T) -> R): R
```
use() 함수는 객체를 사용한 후 close() 함수를 자동적으로 호출해 닫아 줄 수 있습니다. 
내부 구현을 보면 예외 오류 발생 여부와 상관없이 항상 close() 함수 호출을 보장합니다.
```kotlin
fun main() {
    PrintWriter(FileOutputStream("d://text//output.txt")).use { 
        it.println("hello")
    }
}
```
파일 작업을 하고 나면 close()를 명시적으로 호출해야 하는데  use 블록 안에서는 그럴 필요가 없습니다.
