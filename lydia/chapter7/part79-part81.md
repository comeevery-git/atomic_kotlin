#### 챕터7. 파워 툴

> 컴퓨터가 이해할 수 있는 코드를 작성하는 건 바보도 할 수 있다.
> 
> 좋은 프로그래머는 사람이 이해할 수 있는 코드를 작성한다. 
>
> - 마틴 파울러(Martin Fowler)

---


## 79. 확장 람다
- 확장 람다는 확장 함수와 비슷하다. 차이가 있다면 함수가 아니라 람다라는 점이다.
- 다음 코드에서 va와 vb는 같은 결과를 내놓는다.
```kotlin
val va: (String, Int) -> String = {str, n -> // 일반 람다
  str.repeat(n) + str.repeat(n)
}
val vb: String.(Int) -> String = { // String 파라미터를 괄호 밖으로 옮겨서 String.(Int)처럼 확장 함수 구문 사용
  this.repeat(it) + repeat(it) // this를 통해 수신 객체에 접근
}
  
fun main() {
  va("Vanbo", 2) eq "VanboVanboVanboVanbo"
  "Vanbo".vb(2) eq "VanboVanboVanboVanbo"
  vb("Vanbo", 2) eq "VanboVanboVanboVanbo"
  // "Vanbo".va(2) // Compile error
}
```
- 코틀린 문서에서는 확장 람다를 `수신 객체가 지정된 함수 리터럴` 이라고 말한다.
  - `함수 리터럴` 이라는 말은 람다와 익명 함수를 모두 포함한다.
  - 수신 객체가 `암시적 파라미터로 추가 지정된 람다`라는 사실을 강조하고 싶을 때는
    - 수신 객체가 지정된 람다를 `확장 람다`의 동의어로 더 자주 사용한다.
- 확장 함수와 마찬가지로 확장 람다도 여러 파라미터를 받을 수 있다.

### 참고링크
- [코틀린 공식 문서 - 수신 객체가 지정된 함수 리터럴](https://kotlinlang.org/docs/lambdas.html#function-literals-with-receiver)
- [코틀린 공식 문서 - 확장 람다](https://kotlinlang.org/docs/lambdas.html#extension-lambdas)
- [코틀린 공식 문서 - 확장 함수](https://kotlinlang.org/docs/extensions.html#extension-functions)
- [확장 함수, 람다 함수, 고차 함수의 기초](https://mashup-android.vercel.app/mashup-11th/Ahnseokjoo/baselambda/BaseLambda)

---


## 80. 영역 함수(Scope Functions)
- 영역 함수는 객체의 이름을 사용하지 않아도 그 객체에 접근할 수 있는 임시 영역을 만들어주는 함수다.
  - let(), run(), with(), apply(), also() 라는 다섯가지 영역 함수가 있다.
  - 각각은 람다와 함께 쓰이며, 따로 임포트 할 필요가 없다.
- 각 영역 함수는 여러분이 `문맥 객체`를 it 으로 다루는 지, 혹은 this 로 다루는지와 각 함수가 어떤 값을 반환하는지에 따라 달라진다.
  - with 만 나머지 네 함수와 다른 호출 문법을 사용한다.
  - let, run, with 는 람다의 결과를 반환한다.
    - run 은 확장함수, with 은 일반함수
  - apply, also 는 객체 자신을 반환한다.
```kotlin
fun main() {
  val str = "Hello"
  str.run { // run은 수신 객체를 람다의 리시버로 전달하고 람다의 결과를 반환한다.
    println("The receiver string length: $length")
    println("The receiver string length: ${this.length}") // this를 통해 수신 객체에 접근
  }
  str.let { // let은 수신 객체를 람다의 인자로 전달하고 람다의 결과를 반환한다. it을 통해 수신 객체에 접근
    println("The receiver string's length is ${it.length}")
  }
  with(str) { // with은 run과 비슷하지만 결과를 반환하지 않는다.
    println("The receiver string's length is ${length}") // this를 통해 수신 객체에 접근. 암시적 this
  }
  str.apply { // apply는 수신 객체를 람다의 리시버로 전달하고 객체 자신을 반환한다.
    println("The receiver string's length is ${length}") // this를 통해 수신 객체에 접근. 암시적 this
  }
  str.also { // also는 apply와 비슷하지만 객체 자신을 반환한다.
    println("The receiver string's length is ${it.length}") // it을 통해 수신 객체에 접근
  }
}
```
- 영역함수는 인라인 된다.
  - 일반적으로 람다를 인자로 전달하면, 람다 코드를 외부 객체에 넣기 때문에 일반 함수 호출에 비해 실행 시점의 부가 비용이 좀 더 발생한다.
    - 람다가 주는 장점에 비하면 이런 부가 비용은 큰 문제가 되지 않고, JVM에는 부가 비용을 상쇄해줄 만한 여러 최적화 기능이 있다.
    - 하지만 람다를 인자로 받는 함수가 인라인 함수라면, 모든 실행 시점 부가 비용을 없앨 수 있다.
    - 인라인 함수가 람다를 인자로 받으면, 컴파일러는 인라인 함수의 본문과 함께 람다 본문을 인라인 해준다.
    - 그러면 람다 본문이 외부 객체에 들어가는 것이 아니라, 인라인 함수 본문에 직접 들어가기 때문에 실행 시점의 부가 비용이 없어진다.
### 참고링크
- [코틀린의 영역함수](https://kotlinlang.org/docs/scope-functions.html#function-selection)


---


## 81. 제네릭스 만들기
- 제네릭스는 `나중에 지정할` 타입에 대해 작동하는 코드를 말한다.
  - 제네릭스를 사용하면 타입을 지정하지 않고도 코드를 작성할 수 있다.
### Any
  - 코틀린의 모든 클래스는 Any 클래스를 상속한다.
  - Any 클래스는 코틀린의 최상위 클래스다.
    - Any 클래스는 자바의 Object 클래스와 같다.
  - Any 클래스는 equals(), hashCode(), toString() 등의 메소드를 제공한다.
    - 이 메소드들은 모든 클래스에서 사용할 수 있다.
  - 미리 정해지지 않은 타입을 다루는 방법 중 하나로 Any 타입의 인자를 전달하는 방법이 있고, 때로 이를 제네릭스를 사용해야 하는 경우와 혼동하기도 한다.
- 타입을 변환할 때 잘못된 타입을 지정하면 런타임 오류가 발생할 가능성이 있다(그리고 성능도 약간 나빠진다)
  - 이런 문제를 피하려면 타입을 변환할 때 is 키워드를 사용해 타입을 검사하고, as 키워드를 사용해 타입을 변환한다.
  - 또는 제네릭스를 사용해 타입을 변환할 수 있다.
```kotlin
fun main() {
  class Person {
    fun speak() = "Hi!"
  }
  class Dog {
    fun bark() = "Ruff!"
  }
  class Robot {
    fun communicate() = "Beep!"
  }
  
  fun talk(speaker: Any) = when (speaker) {
    is Person -> speaker.speak()
    is Dog -> speaker.bark()
    is Robot -> speaker.communicate()
    else -> "Not a talker" // 또는 예외 처리
  }
  
  fun main() {
    talk(Person()) eq "Hi!"
    talk(Dog()) eq "Ruff!"
    talk(Robot()) eq "Beep!"
    talk(1) eq "Not a talker"
  }
}
```
### 제네릭스 정의하기
  - 중복된 코드는 제네릭 함수나 타입으로 변환하는 것을 고려해볼만 하다.
```kotlin
class Robot {
  fun communicate() = "Beep!"
}
class Communicator<T> (val input: T) { // 제네릭 클래스
  fun communicate() = when (input) {
    is String -> "You said: $input"
    is Int -> "You gave the Int: $input"
    is Robot -> input.communicate()
    else -> "I don't understand you"
  }
}

fun main() {
  Communicator("Hello").communicate() eq "You said: Hello"
  Communicator(123).communicate() eq "You gave the Int: 123"
  Communicator(Robot()).communicate() eq "Beep!"
  Communicator(12.34).communicate() eq "I don't understand you"
}
```
### 타입 정보 보존하기
  - 제네릭 클래스나 제네릭 함수의 내부 코드는 T 타입에 대해 알 수 없다.
    - 따라서 T 타입의 인스턴스를 다룰 때는 T 타입에 대해 알 수 있는 방법이 필요하다.
    - 이런 방법 중 하나가 타입 파라미터 제약이다.
  - 제네릭스는 반환값의 타입 정보를 유지하는 방법이라고 생각할 수 있다.
    - 이런 방식을 사용하면 반환값이 원하는 타입인 지 명시적으로 검사하고 변환할 필요가 없어진다.
### 타입 파라미터 제약(Types Parameter Constraints)
  - 제네릭 타입 인자가 다른 클래스를 상속해야 한다고 지정한다.
  - 타입 파라미터 제약을 사용하면 제네릭 타입 인자가 특정 클래스를 상속하거나 특정 인터페이스를 구현해야 한다고 지정할 수 있다.
  - 타입 파라미터 제약이 필요한 경우는 다음 두 가지가 `모두` 필요할 때 뿐이다.
    - 타입 파라미터 안에 선언된 함수나 프로퍼티에 접근해야 한다.
    - 결과를 반환할 때 타입을 유지해야 한다.
### 타입 소거(Type erasure)
  - 코틀린은 제네릭 타입 인자를 컴파일 시점에 검사하고, 실행 시점에는 타입 인자를 알 수 없다.
    - 이는 다음의 두 가지 이유 때문이다.
      1. 자바와의 호환성
         - 최초의 자바에는 제네릭이 포함되어 있지 않았고, 면년 후 제네릭스가 추가되었지만 이미 엄청나게 많은 코드가 제네릭스 없이 작성된 상태였다.
         - 제네릭스를 도입하면서 기존 코드를 깨지않는 절충점이 아주 중요했다.
      2. 타입 정보를 유지하려면 부가 비용이 든다.
         - 제네릭 타입 정보를 저장하면 제네릭 List나 Map이 차지하는 메모리가 상당히 늘어난다.
           - 예를들어 표준 Map은 Map.Entry 객체로 이뤄져있는데, Map.Entry는 제네릭 객체다.
             - 따라서 제네릭 객체가 모든 곳에서 타입 파라미터를 실체화해 저장한다면, Map.Entry의 모든 키와 값은 부가 타입 정보를 저장해야 하므로 메모리 사용량이 크게 늘어난다.
### 함수의 타입 인자에 대한 실체화
  - 제네릭 함수를 호출할 때도 타입 정보가 소거된다.
    - 함수 안에서는 제네릭 타입 파라미터를 사용해 할 수 있는 일이 별로 없다.
  - 함수 인자의 타입 정보를 보존하려면 reified 키워드를 추가해야 한다.
    - 함수 a()가 작업을 수행하기 위해서는 클래스 정보가 필요한다고 가정하자.
```kotlin
import kotlin.reflect.KClass

fun <T: Any> a(kClass: KClass<T>) {
  println(kClass.simpleName)
}
```
- a() 를 두번째 제네릭 함수인 b() 에서 호출한다면 제네릭 인자의 타입 정보를 전달하고 싶을 것이다.
```kotlin
import kotlin.reflect.KClass

// 타입 소거로 인해 컴파일 되지않음
// fun <T: Any> b() = a(T::class)

fun <T: Any> c(kClass: KClass<T>) = a(kClass)

class K

val kc = c(K::class)
```
- 컴파일러가 이미 T의 타입을 알고 조용히 타입을 넘겨줄 수 있는데, 이런 식으로 명시적으로 타입 정보를 전달하는 것은 불필요한 중복임에 `틀림없다`.
  - 실제 reified 키워드가 해주는 일이 바로 이런 일이다.
  - reified를 사용하기 위해서는 제네릭 함수를 inline 으로 선언해야 한다.
    - 라이브러리 함수인 filterIsInstance()도 reified 키워드를 사용해 정의되어 있다.
      - filterIsInstance()란 특정 타입의 인스턴스만 필터링하는 함수다.
```kotlin
inline fun <reified T: Any> d() = a(T::class) // reified 키워드를 사용하면 컴파일러가 T의 타입을 알고 있음을 알려준다.
val kd = d<K>() // K::class 를 전달하지 않아도 된다.
```
- d()는 c()와 같은 효과를 내지만, 클래스 참조를 인자로 요구할 필요가 없다.
- 이제 실행 시점에도 타입 정보를 사용할 수 있기 때문에 함수 본문 안에서 이를 쓸 수 있다.
- 실체화를 사용하면 is를 제네릭 파라미터 타입에 적용할 수 있다.
```kotlin
inline fun <reified T> check(t: Any) = t is T

// fun <T> check(t: Any) = t is T // 컴파일 에러

fun main() {
  check<String>("1") eq true
  check<Int>("1") eq false
}
```
### 타입 변성
- 타입 변성이란?
  - 타입 변성은 제네릭 타입의 서브 타입 관계를 정의한다.
  - `Container<T>` 라는 제네릭 타입 객체를 `Container<U>`라는 제네릭 타입 컨테이너 객체에 대입하고 싶다고 한다면
    - in 이나 out 변성 에너테이션(variance annotation)을 붙여서 타입 파라미터를 적용한 Container 타입의 상하위 타입 관계를 제한해야 한다.
    - in 변성은 Container<out T> 타입을 의미한다.
      - Container<out T> 타입은 T의 서브 타입이다.
      - 따라서 Container<out T> 타입의 인스턴스는 T 타입의 값을 반환하는 메소드를 가질 수 있다.
      - 하지만 T 타입의 값을 받는 메소드는 가질 수 없다.
    - out 변성은 Container<in T> 타입을 의미한다.
      - Container<in T> 타입은 T의 슈퍼 타입이다.
      - 따라서 Container<in T> 타입의 인스턴스는 T 타입의 값을 받는 메소드를 가질 수 있다.
      - 하지만 T 타입의 값을 반환하는 메소드는 가질 수 없다.
```kotlin
class Box<T>(private var contents: T) {
  fun put(item: T) { contents = item }
  fun get(): T = contents
}
class InBox(In T)(private var contents: T) { // in 변성
  fun put(item: T) { contents = item }
}
class OutBox(Out T)(private var contents: T) { // out 변성
  fun get(): T = contents
}
```
- 왜 이런 제약이 필요할까?
```kotlin
open class Pet
class Cat : Pet()
class Dog : Pet()
```
- Cat과 Dog는 Pet의 서브 타입이다.
  - 따라서 Cat과 Dog를 Pet 타입의 변수에 할당할 수 있다.
```kotlin
val catBox = Box<Cat>(Cat())
// val petBox: Box<Pet> = catBox // 컴파일 에러
// val anyBox: Box<Any> = catBox // 컴파일 에러
```
- 코틀린이 위 코드를 허용한다면 petBox에는 put(item: Pet)이라는 함수가 있을 것이다.
- Dog도 Pet 이므로 이를 허용하면 Dog을 catBox에 넣을 수 있게 되는데, 이는 catBox가 고양이 박스라는 점을 위반하게 된다.
- in 변성은 Pet의 슈퍼 타입인 Any를 받는 put(item: Any) 함수를 가질 수 있다.
  - 이 함수는 Cat이나 Dog를 받을 수 있으므로 catBox에 put()을 호출할 수 있다.
  - 하지만 catBox는 고양이 박스이므로 이는 위반된다.
- 타입 안정성(type safety)을 위해 코틀린은 제네릭 타입의 서브 타입 관계를 제한한다.
- 함수는 `공변적인 반환 타입`을 가진다.
  - 공변적인 반환 타입은 서브 타입 관계를 유지한다.
  - 오버라이드하는 함수가 오버라이드 대상 함수보다 더 구체적인 반호나 타입을 돌려줘도 된다는 뜻이다.
```kotlin
interface Parent
interface Child: Parent

interface X {
  fun f(): Parent
}
interface Y : X {
  override fun f(): Child
}
```
- X 인터페이스의 f() 함수는 Parent를 반환한다.
  - Y 인터페이스는 X 인터페이스를 상속하므로 f() 함수를 오버라이드할 수 있다.
  - Y 인터페이스의 f() 함수는 Child를 반환한다.
  - 이는 X 인터페이스의 f() 함수가 반환하는 타입보다 더 구체적인 타입을 반환한다는 뜻이다.
  - 이런 경우를 `공변적인 반환 타입`이라고 한다.
### 참고링크
- [제네릭스 관련 블로그](https://readystory.tistory.com/201)


---


