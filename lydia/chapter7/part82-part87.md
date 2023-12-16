#### 챕터7. 파워 툴

---


## 82. 연산자 오버로딩(Operator Overloading)
- 컴퓨터 프로그래밍에서 오버로딩(overloading)은 다형성의 한 종류로, 같은 이름의 메소드를 여러 개 가지면서 매개변수의 유형과 개수를 다르게 하여 다양한 상황에 대처할 수 있게 하는 기술이다.
    - 이미 존재하는 어떤 대상에 추가로 의미를 더하는 것 을 뜻한다.
- 오버로딩의 과거는 조금 복잡한데, 연산자 오버로딩은 C++에서 유명해졌지만 C++에는 Garbage Collection이 없었다.
  - 오버로딩한 연산자를 작성하는 게 어려웠다.
    - 그래서 초기 자바 설계자들은 연산자 오버로딩이 `나쁘다`라는 결론을 내렸고, 자바에서는 연산자 오버로딩 구현이 상대적으로 쉬움에도 이를 허용하지 않았다.
- 코틀린은 다양한 언어들로부터 배운 내용을 바탕으로 연산자 오버로딩의 과정을 단순화하고, 익숙하거나 오버로딩하는 것이 타당한 몇몇 연산자만 선택해서 오버로딩할 수 있도록 선택지를 제한했다.
  - 게다가 코틀린에서는 연산자의 우선순위도 바꿀 수 없다.
```kotlin
data class Num(val n: Int)

operator fun Num.plus(rval: Num) = // Num 타입에 plus 연산자를 추가
    Num(n + rval.n)

fun main() {
    Num(4) + Num(5) eq Num(9)
    Num(4).plus(Num(5)) eq Num(9)
}
```
- 두 피연산자 사이에 사용하기 위해 일반(연산자가 아닌) 함수를 정의하고 싶다면 infix 키워드를 사용하면 된다.
  - 하지만 연산자들은 (대부분) 이미 infix이므로 infix를 붙이지 않아도 된다.
```kotlin
infix fun Num.plus(rval: Num) = // infix 키워드를 사용해 Num 타입에 plus 연산자를 추가
    Num(n + rval.n)
```
> <b>Java와 Kotlin의 오버로딩 차이점</b>
> - Java는 오버로딩을 정적으로 처리한다.
>   - 즉, `컴파일 시점`에 오버로딩된 메소드를 선택한다.
>   - 오버로딩된 메소드는 컴파일 시점에 이미 결정되어 있다.
>   - 따라서 오버로딩된 메소드는 실행 시점에 변경될 수 `없다.`
> - Kotlin은 오버로딩을 동적으로 처리한다.
>   - 즉, `런타임 시점`에 오버로딩된 메소드를 선택한다.
>   - 오버로딩된 메소드는 실행 시점에 결정된다.
>   - 따라서 오버로딩된 메소드는 실행 시점에 변경될 수 `있다.`

### 동등성(Equality)
- 동등성(==)과 비동등성(!=)은 equals() 멤버 함수를 호출한다.
  - data 클래스는 자동으로 저장된 모든 필드를 서로 비교하는 equals()를 오버라이드 해준다.
  - equals()를 오버라이드 하지 않은 클래스는 자바와 마찬가지로 참조 동등성을 사용한다.
    - 참조 동등성은 a, b가 같은 객체인 지 검사할 때 a === b를 사용한다.
      - 메모리에서 같은 위치를 가리키는지 검사한다.
  - equals()는 확장 함수로 정의할 수 없는 유일한 연산자다.
    - 클래스 안에서 equals()를 정의할 때는 디폴트 equals(other: Any?)를 오버라이드한다.
  - equals()를 오버라이드할 때 항상 hashCode()도 오버라이드해야 한다.
    - hashCode()는 객체를 해시 테이블에 저장할 때 사용하는 해시 코드를 반환한다.
    - equals()가 같은 객체에 대해 항상 같은 값을 반환하도록 hashCode()를 오버라이드해야 한다.

### 산술 연산자(Arithmetic Operators)
- class E에 대해 기본 산술 연산자(+, -, *, /, %)를 확장으로 정의할 수 있다.
- 산술 연산자의 우선순위는 자바와 같다.
  - 우선순위를 분명히 알 수 없는 여러 연산자를 섞어 쓸 때는(물론 컴파일러는 우선순위를 분명히 알고있다) 괄호를 사용하는 것이 좋다.

### 비교 연산자(Comparison Operators)
- compareTo()를 정의하면 모든 비교 연산자(<, >, <=, >=)를 쓸 수 있다.
- compareTo()는 다음을 알려주는 Int를 반환해야 한다.
  - 두 피연산자(a, b)가 동등한 경우 0을 반환한다.
  - 첫 번째 피연산자(수신 객체, a)가 두 번째 피연산자(함수의 인자, b)보다 작은 경우 음수를 반환한다.
  - a가 b보다 큰 경우 양수를 반환한다.

### 범위와 컨테이너(Ranges and Containers)
- rangeTo()는 범위를 생성하는 .. 연산자를 오버로드하고, contains()는 값이 범위 안에 들어가는 지 여부를 알려주는 in 연산을 오버로드한다.
- 범위는 Comparable 인터페이스를 구현하는 클래스에 대해 정의할 수 있다.
  - Comparable 인터페이스는 compareTo()를 정의한다.

### 컨테이너 원소 접근(Container Element Access)
- get()과 set()은 각괄호([])를 사용해 컨테이너의 원소를 읽고 쓰는 연산을 정의한다.
```kotlin
operator fun Point.get(index: Int): Int {
    return when (index) {
        0 -> x
        1 -> y
        else -> throw IndexOutOfBoundsException("Invalid coordinate $index")
    }
}

operator fun Point.set(index: Int, value: Int) {
    when (index) {
        0 -> x = value
        1 -> y = value
        else -> throw IndexOutOfBoundsException("Invalid coordinate $index")
    }
}

fun main() {
    val p = Point(10, 20) // Point(x=10, y=20)
    p[1] eq 20 // p.get(1) 과 같다.
    p[0] = 42 // p.set(0, 42) 와 같다.
    p eq Point(42, 20)
}
```
### 호출 연산자(Invoke Operator)
- 객체 참조 뒤에 괄호를 넣으면 invoke()를 호출한다.
  - invoke()는 함수처럼 호출할 수 있는 객체를 정의할 때 사용한다.
  - invoke()가 받을 수 있는 파라미터 개수는 원하는 대로 정할 수 있다.
```kotlin
operator fun Point.invoke(): String { // invoke()를 오버로드
    return "($x, $y)"
}

fun main() {
    val p = Point(10, 20) // Point(x=10, y=20)
    p() eq "(10, 20)" // p.invoke()
}
```
- invoke()를 확장 함수로 정의할 수도 있다.
  - invoke()를 확장 함수로 정의하면 객체 참조 뒤에 괄호를 넣어 호출할 수 있다.
- 함수참조가 있는 경우 이 함수참조를 invoke()를 사용해 호출할 수도 있고, 괄호를 사용해 호출할 수도 있다.
```kotlin
fun main() {
    val func: (String) -> Int = { it.length } // 람다를 사용해 함수 참조를 만든다.
    func("abc") eq 3
    func.invoke("abc") eq 3
    func("abc") eq func.invoke("abc")
    val nullableFunc: ((String) -> Int)? = func
    nullableFunc?.invoke("abc") eq 3 // 함수 참조가 널이 될 수 있는 함수 참조라면 invoke()와 안전한 접근을 함께 사용해야 한다.
}
```
### 역작은따옴표로 감싼 함수 이름
- 코틀린은 함수 이름을 역작은따옴표(`)로 감싸는 경우, 함수 이름에 공백, 몇몇 비표준 글자, 예약어 등을 사용하는 것을 허용한다.
  - 역작은따옴표로 감싼 함수 이름은 함수 참조를 만들 때 유용하다.
    - 함수 참조를 만드는 이유는 함수를 값으로 사용하기 위해서다.
    - 또한 테스트 시 읽기 쉬운 이름의 테스트 함수를 정의할 수 있으므로 테스트 코드를 작성할 때도 유용하다.
    - 역작은따옴표로 정의한 이름은 자바 코드와의 상호 작용도 더 쉽게 해준다.
```kotlin
fun `plus`(p: Point): Point {
    return Point(x + p.x, y + p.y)
}

fun main() {
    val p = Point(10, 20)
    val plus = p::`plus` // 함수 참조를 만들 때 역작은따옴표를 사용
    plus(Point(30, 40)) eq Point(40, 60)
}
```


---


## 83. 연산자 사용하기
- 실전에서 연산자를 오버로드하는 경우는 드물다. 연산자 오버로드는 보통 직접 라이브러리를 만들 때만 사용한다.
  - 연산자 오버로드는 편리한 문법이지만, 오버로드를 남용하면 코드를 읽기 어려워진다.
  - 그리고 종종 주기적으로, 사용 중인지 눈치채지도 못한 채 오버로드한 연산자를 사용하기도 한다.
- 예를들어 코틀린 라이브러리에는 컬렉션 처리를 손쉽게 해주는 수많은 연산자 정의가 들어있다.
```kotlin
fun main() {
    val list = MutableList(10) { 'a' + it }
    list[7] eq 'h' // operator get()
    list.get(8) eq 'i' // 명시적 호출
    list[7] = 'z' // operator set()
    list.set(8, 'j') // 명시적 호출
    list += 'k' // list.add('k') 와 같다.
    list -= 'b' // list.remove('b') 와 같다.
    ('d' in list) eq true // operator contains()
    list.contains('e') eq true // 명시적 호출
}
```
- 가변 컬렉션에 += 을 호출하면 컬렉션 내용을 변경하지만, +를 호출하면 예전 원소에 새 원소가 추가된 새로운 컬렉션을 반환한다.
- 읽기 전용 컬렉션에 += 을 호출하면 예상하는 결과를 얻을 수 없을지도 모른다.
```kotlin
fun main() {
    val list = listOf(1, 2)
    list += 3 // 예상과 다를 수 있음
    list eq "[1, 2, 3]" // list = list + 3 과 같다.
}
```
- 가변 컬렉션에서 a += b 는 a를 변경하는 plusAssign()을 호출한다.
- 읽기 전용 컬렉션에서는 plusAssign()이 들어있지 않다.
  - 따라서 코틀린은 a += b를 a + b 로 해석한다.

### 구조분해 연산자
- 보통 직접 정의할 일이 거의 없는 또 다른 연산자로, 구조 분해에 사용되는 componantN() 함수를 말한다.
  - componant1, componant2, componant3 등
- 구조 분해는 객체의 여러 프로퍼티를 한 번에 여러 변수에 할당하는 것을 말한다.
```kotlin
class Point(val x: Int, val y: Int) {
    operator fun component1() = x
    operator fun component2() = y
}

fun main() {
    val p = Point(10, 20)
    val (x, y) = p // 구조 분해
    x eq 10
    y eq 20
}
```
- data 클래스는 자동으로 componantN() 함수를 만들어준다.
  - 코틀린은 data 클래스의 각 프로퍼티에 대해(data 클래스 생성자에 프로퍼티가 나타난 순서대로) componantN을 생성해준다.


---


## 84. 프로퍼티 위임
- 프로퍼티는 접근자 로직을 위임할 수 있다.
  - `by` 키워드를 사용하면 프로퍼티를 위임과 연결할 수 있다.
> val(또는 var) 프로퍼티이름 by 위임객체이름
- 프로퍼티가 val(읽기 전용)인 경우 위임 객체의 클래스에는 getValue() 함수 정의가 있어야 한다.
- 프로퍼티가 var(읽고 쓰기 가능)인 경우 위임 객체의 클래스에는 getValue()와 setValue() 함수 정의가 있어야 한다.
```kotlin
class Name {
    var value: String by Delegate() // Delegate() 객체를 사용해 프로퍼티 접근자 로직을 위임한다.
}

class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String { // getValue() 함수 정의
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) { // setValue() 함수 정의
        println("$value has been assigned to '${property.name} in $thisRef.'")
    }
}

fun main() {
    val name = Name()
    name.value = "Lydia" // setValue() 함수 호출
    name.value eq "Lydia" // getValue() 함수 호출
    
    var delegate = Delegate()
    delegate.value = "Lydia" // setValue() 함수 호출
    delegate.value eq "Lydia" // getValue() 함수 호출
}
```
- 위임 객체는 프로퍼티의 값을 저장하거나, 프로퍼티에 접근할 때 추가적인 로직을 수행할 수 있다.
  - 프로퍼티의 값을 저장하는 위임 객체를 백킹 필드(backing field)라고 부른다.
- SAM 변환을 사용해 getValue()와 setValue()를 정의할 수 있다.
```kotlin
class Add(val a: Int, val b: Int) {
  val sum by Sum()
}

class Sum

operator fun Sum.getValue(
  thisRef: Add,
  property: KProperty<*>
) = thisRef.a + thisRef.b

fun main() {
  val add = Add(2, 3)
  add.sum eq 5
}
```
- 이 방식을 사용하면 변경하거나 상속할 수 없는 기존 클래스에 getValue()와 setValue()를 추가할 수 있어서 이 클래스의 인스턴스를 위임 객체로 사용할 수 있게 된다.
  - 이런 방식으로 코틀린 표준 라이브러리에는 Lazy, Observable, Vetoable 등의 위임 객체가 들어있다.
  - Lazy는 프로퍼티를 처음 사용할 때까지 초기화를 미루는 위임 객체다.
  - Observable은 프로퍼티의 값이 바뀔 때마다 이벤트를 발생시키는 위임 객체다.
  - Vetoable은 프로퍼티의 값을 변경하기 전에 이를 거부할 수 있는 위임 객체다.
    - 이런 위임 객체를 사용하면 프로퍼티의 접근자 로직을 쉽게 재사용할 수 있다.


---


## 프로퍼티 위임 도구
- 표준 라이브러리에는 특별한 프로퍼티 위임 연산이 들어있다.
- Map은 위임 프로퍼티의 위임 객체로 쓰일 수 있도록 미리 설정된 코틀린 표준 라이브러리 타입 중 하나다.
  - 어떤 클래스의 모든 프로퍼티를 저장하기 위해 Map을 하나만 써도 된다.
    - 이 맵에서 각 프로퍼티는 String 타입의 키가 되고, 저장한 값은 맵에서 키와 연관된 값이 된다.
```kotlin
class Driver(
  map: MutableMap<String, Any?>
) {
  var name: String by map
  var age: Int by map
  var id: String by map
  var available: Boolean by map
  var coord: Pair<Double, Double> by map
}

fun main() {
  val info = mutableMapOf<String, Any?>(
    "name" to "Lydia",
    "age" to 21,
    "id" to "1234567890",
    "available" to false,
    "coord" to Pair(51.5074, 0.1278)
  )
  
  val driver = Driver(info)
  driver.available eq false
  driver.available = true
  info eq "{name=Lydia, " +
    "age=21, id=1234567890, " +
    "available=true, coord=(51.5074, 0.1278)}"
}
```
- 주의할 점은 driver.available = true 라고 설정할 때 원본맵인 info가 변경된다는 점이다.
- 이런 방식으로 Map을 사용하면 클래스의 인스턴스를 직렬화하거나, 데이터베이스에 저장하거나, 네트워크를 통해 전송할 때 유용하다.
  - 프로퍼티의 이름을 키로 사용하고, 프로퍼티의 값을 값으로 사용하는 Map을 사용하면 된다.
- 이런 동작이 작동하는 이유는 코틀린 표준 라이브러리에서 Map의 확장 함수로 프로퍼티 위임을 가능하게 해주는 getValue()와 setValue()를 제공하기 때문이다.
```kotlin
operator fun <T> MutableMap<String, T>.getValue( // getValue() 함수 정의
  thisRef: Any?, // 위임 객체
  property: KProperty<*> // 위임 프로퍼티
): T { // 반환 타입은 프로퍼티와 같아야 한다.
  return this[property.name] as T
}

operator fun <T> MutableMap<String, T>.setValue( // setValue() 함수 정의
  thisRef: Any?, // 위임 객체
  property: KProperty<*>, // 위임 프로퍼티
  value: T // 프로퍼티에 새로 할당할 값
) {
  this[property.name] = value
}
```


---


