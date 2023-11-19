#### 챕터2. 객체 소개
- 코틀린은 자바와 같은 객체지향 언어와 함수형 언어의 장점을 모두 가지고 있다.


## 16. 객체는 모든 곳에 존재한다
- 객체는 프로퍼티(val과 var)를 사용해 데이터를 저장하고, 함수를 사용해 이런 데이터에 대한 연산을 수행한다.
```kotlin
fun main() {
    val r1 = IntRange(0, 10)
    val r2 = 5..7
    println(r1)
    println(r2)
}
```
- 위 예제는 IntRange 클래스에 속하는 두 객체(인스턴스)를 만든다.
  - 각 객체는 메모리에 자신만의 저장공간을 가지고 있다.
  - IntRange 는 클래스지만, 0부터 10까지의 범위를 가리키는 r1 범위는 r2 와 구분되는 별도의 객체다. 


***


## 17. 클래스 만들기
- IntRange 나 String 처럼 미리 정의된 타입을 사용할 수 있을 뿐만 아니라, 직접 원하는 객체의 타입을 정의할 수도 있다.
  - 멤버 함수: 클래스에 속한 함수
  - 최상위(top-level) 함수: 클래스에 속하지 않은 함수
```kotlin
class Dog {
    fun bark() {
        println("Yip!")
    }
}
fun main() {
    val dog = Dog()
}
```
- main() 에서는 Dog 객체를 만들어서 dog 이라는 val 에 대입한다.
  - dog을 사용하지 않으면 경고가 표시된다.
```kotlin
class Cat {
    fun meow() = "mrrrow!"
}
fun main() {
    val cat = Cat()
    val m1 = cat.meow()
    println(m1)
    // mrrrow!
}
```
- 객체 내에서는 this 를 사용해 자신의 프로퍼티와 멤버 함수에 접근할 수 있다.
  - this 를 반드시 적어야 하거나 this 를 적는 스타일을 권장하는 프로그래머도 있다.
  - 코틀린에서는 this 를 생략할 수 있고, 불필요한 this는 사용하지 않는 것이 좋다.


***


## 18. 프로퍼티
- 프로퍼티(property)는 클래스에 속한 var 나 val 이다.
  - 프로퍼티를 정의함으로써 클래스 안에서 상태를 유지한다.
  - 함수를 한두 개 별도로 작성하는 대신에 클래스를 작성하는 주된 이유가 바로 상태 유지다.
```kotlin
val constant = 42
var counter = 0

fun increment() {
    counter++
}
```
- 변경할 수 없으므로 최상위 수준에 val 을 정의해도 안전하다.
  - 하지만 가변(mutable)인 최상위 프로퍼티를 선언하는 일은 안티 패턴으로 간주된다.
  - 프로그램이 복잡해질수록 공유된 가변 상태에 대해 제대로 추론하기 어려워지기 때문이다.
  - 가변 상태를 클래스 안에 가두는 것이 가장 좋다. (이후 가시성 제한하기 파트에서 다룬다.)

- var 과 val 은 객체가 아니라 참조를 제한한다.
  - var 를 사용하면 참조가 가리키는 대상을 다른 대상으로 다시 엮을 수 있지만,
  - val 를 사용하면 다른 대상을 엮을 수 없다.
***


## 19. 생성자
- 생성자(constructor)에 정보를 전달해 새 객체를 초기화 할 수 있다.
```kotlin
class Wombat
fun main() {
    val wombat = Wombat()
}
```
- 다른 객체 지향 언어에서는 new 키워드가 필요했을 수 있다.
  - 코틀린은 new가 불필요한 중복이기 때문에 제외한다.
- 생성자 파라미터 목록에 있는 name을 var나 val로 정의하면 해당 식별자가 프로퍼티로 바뀌며, 생성자 밖에서도 이 식별자에 접근할 수 있게된다.
  - var 로 정의한 생성자 파라미터는 가변 프로퍼티가 된다.
```kotlin
class AlienSpecies(
    val name: String,
    var eyes: Int
) {
    fun printInfo() {
        println("name: $name, eyes: $eyes")
    }
}

fun main() {
  val alienSpecies = AlienSpecies("Quetzalcoatlus", 2)
  println(alienSpecies) // AlienSpecies@3b9a45b3
  alienSpecies.printInfo() // name: Quetzalcoatlus, eyes: 2
}
```
- println() 은 문자열 대신 객체를 전달받은 경우 객체의 toString()을 호출한 결과를 출력한다.
- 클래스에 toString()을 정의하지 않으면, 클래스 이름과 객체의 해시 코드를 출력한다.
  - 클래스에 직접 toString()을 정의하지 않으면 디폴트 toString() 이 호출된다.
```kotlin
// 직접 toString()을 구현하는 경우
class AlienSpecies(
    val name: String,
    var eyes: Int
) {
    override fun toString(): String {
        return "AlienSpecies(name='$name', eyes=$eyes)"
    }
}
fun main() {
  val alienSpecies = AlienSpecies("Quetzalcoatlus", 2)
  println(alienSpecies) // AlienSpecies(name='Quetzalcoatlus', eyes=2)
}
```
- override 키워드는 이미 정의된 toString() 메서드의 정의를 대신할 진짜 새 정의를 제공하겠다는 뜻이다.
  - override 키워드를 사용함으로써 코드의 의도를 명확히 할 수 있고, 실수를 줄일 수 있다.


***


## 20. 가시성 제한하기
- 변화해야하는 요소와 동일하게 유지되어야 하는 요소를 분리하라.
  - `접근 변경자(access modifier)` 는 클래스의 프로퍼티와 멤버 함수에 대한 접근을 제한한다.
  - **클래스의 프로퍼티와 멤버 함수는 기본적으로 `public` 이다.**
    - public 으로 정의된 프로퍼티와 멤버 함수는 어디서나 접근할 수 있다.
    - public 으로 정의된 클래스는 어디서나 접근할 수 있다.
  - **`private`, `protected` 으로 정의된 프로퍼티와 멤버 함수는 클래스 외부에서 접근할 수 없다.**
    - private, protected 으로 정의된 클래스는 같은 파일 안에서만 접근할 수 있다.
    - protected 와 private 의 차이점은 상속을 통해 클래스를 확장할 때 드러난다.
      - private 으로 정의된 프로퍼티와 멤버 함수는 하위 클래스에서 접근할 수 없다.
      - protected 로 정의된 프로퍼티와 멤버 함수는 하위 클래스에서 접근할 수 있다.
  - **모듈(`module`)은 코틀린 컴파일러가 한 번에 컴파일하는 단위다.**
    - 모듈은 하나 이상의 파일로 구성되며, 모듈 안에 있는 모든 파일은 같은 모듈로 간주된다.
    - 모듈은 컴파일 시간에 결정되는 개념이다.
    - 모듈은 런타임에는 존재하지 않는다.
    - **`internal` 로 정의된 프로퍼티와 멤버 함수는 같은 모듈 안에서만 접근할 수 있다.**
      - internal 로 정의된 클래스는 같은 모듈 안에서만 접근할 수 있다.
- 에일리어싱(aliasing)은 프로퍼티나 함수를 다른 이름으로 참조하는 것이다.
  - 에일리어스는 프로퍼티나 함수의 가시성을 변경하지 않는다.


***


## 21. 패키지
- 프로그래밍에서 근본적인 원칙은 DRY, 즉 `반복하지 말라(Don't Repeat Yourself)`는 것이다.
- 패키지(`package`)는 연관 있는 코드를 모아둔 것이다.
  - 예를 들어 Kotlin.math 라이브러리로부터 수학 상수와 함수를 임포트할 수 있다.
  - `as` 키워드를 사용해 임포트한 요소의 이름을 바꿀 수 있다.
  - 파일 이름이 항상 클래스 이름과 같아야하는 자바와 달리, 코틀린에서는 소스 코드 파일 이름으로 아무 이름이나 붙여도 좋다.
  - 별표(`*`)를 사용해 패키지의 모든 요소를 임포트할 수 있다.
```kotlin
import kotlin.math.PI
import kotlin.math.cos
import kotlin.math.sin as sine

fun main() {
    println(PI) // 3.141592653589793
    println(cos(PI)) // -1.0
    println(cos(2 * PI)) // 1.0
    println(sine(PI / 2)) // 1.0
}
```
- 패키지는 코드를 모듈화하고, 코드를 재사용하고, 코드를 캡슐화하는 데 도움이 된다.
- 코드를 재사용하려면 package 키워드를 사용해 패키지를 정의하고, import 키워드를 사용해 재사용할 코드를 임포트한다.


***


