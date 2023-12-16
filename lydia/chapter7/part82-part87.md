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


