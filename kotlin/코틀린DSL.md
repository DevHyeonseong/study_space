## 코틀린 DSL(Domain-Specific Language)

#### DSL(Domain-Specific Language)란?
DSL 이란, 특정 영역을 타겟하고 있는 언어를 말한다. SQL이 대표적인 DSL이다. 

#### 코틀린에서 DSL
코틀린은 범용 언어의 목적을 가지고 있지만 람다식과 확장함수의 개념을 활용하면 코틀린안에서 DSL 형태의 언어를 만들 수 있습니다. 


```kotlin
data class Person(
        var name: String? = null,
        var age: Int? = null,
        var job: Job? = null)

data class Job(
        var category: String? = null,
        var position: String? = null,
        var extension: Int? = null)

/* person함수의 매개변수는 Person클래스의 apply 초기화 블록입니다 */
fun person(block: Person.() -> Unit) : Person = Person().apply(block)

/* job함수는 Person 클래스의 확장 함수 입니다. 매개변수는 Job클래스의 apply 초기화 블록입니다 */
fun Person.job(block: Job.() -> Unit){
    job = Job().apply(block) // == this.job = Job.apply(block)
}

fun main(){
    val person = person { // Person 클래스의 초기화블록을 인자로 person함수를 호출합니다. 
        name = "HyeonSeong"
        age = 24
        job { // Job클래스의 초기화 블록을 인자로 Person.job을 호출합니다
            category = "IT"
            position = "Android Developer"
            extension = 1383
        }
    }
    println(person)
}

//출력 : Person(name=HyeonSeong, age=24, job=Job(category=IT, position=Android Developer, extension=1383))

```

