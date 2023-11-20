#### chapter3. 기초 연습문제

## 30. 확장 함수 (Extension functions)
- 확장 함수란 코틀린에서 기존 클래스에 새로운 함수를 추가하는 방법입니다.
- 확장 함수를 작성할 때는 함수 이름 앞에 수신 객체 타입을 지정하고, 이를 통해 해당 객체의 메서드처럼 호출할 수 있습니다.
- 확장 함수는 기존 클래스의 내부에 있는 메서드가 아니기 때문에 private 멤버에 접근할 수 없습니다.

**문제:** 정수형 리스트의 모든 요소를 곱하는 확장 함수 `multiplyAll`을 작성하십시오.
```kotlin
fun List<Int>.multiplyAll() = this.reduce { acc, num -> acc * num }

fun main() {
    val numbers = listOf(2, 3, 4, 5)
    val product = numbers.multiplyAll()
    println("리스트의 모든 요소 곱: $product")
}
```

---

## 31. 이름 붙은 인자와 기본 인자 (Named and default arguments)
- 이름 붙은 인자와 기본 인자는 함수 호출 시 인자를 지정하는 방법입니다.
- 이름 붙은 인자를 사용하면 함수 호출 시 어떤 인자가 어떤 매개변수에 대응되는지 명시적으로 지정할 수 있습니다.

**문제:** 이름 붙은 인자를 사용하여 함수 printUserInfo를 호출하고, 해당 함수를 호출할 때 기본 인자 값을 변경하십시오.
```kotlin
fun printUserInfo(name: String, age: Int = 30, city: String = "서울") {
    println("이름: $name, 나이: $age, 도시: $city")
}

fun main() {
    printUserInfo(name = "홍길동") // 기본 인자 사용
    printUserInfo(name = "아무개", age = 25, city = "대전") // 이름 붙은 인자 사용
}
```

---

## 32. 오버로딩 (Overloading)
- 오버로딩은 같은 이름의 함수를 여러 번 정의하되, 매개변수의 개수나 타입을 다르게 하는 것입니다.
- 코틀린에서는 함수의 시그니처를 기준으로 오버로딩이 결정됩니다.

**문제:** 덧셈 연산을 오버로딩하여 정수형과 실수형을 더할 수 있는 함수 add를 작성하십시오.
```kotlin
fun add(a: Int, b: Int) = a + b
fun add(a: Double, b: Double) = a + b

fun main() {
    val sum1 = add(3, 5)
    val sum2 = add(2.5, 4.7)
    println("덧셈 결과1: $sum1, 덧셈 결과2: $sum2")
}
```

---

## 33. when 표현식 (when expression)
- when 표현식은 다양한 조건에 따라 다른 동작을 수행하는데 사용됩니다.
- when은 if-else if-else 구문을 대체하여 코드를 간결하게 만들어줍니다.

**문제:** `when` 표현식을 사용하여 학생의 성적을 입력받고, 해당 성적에 대한 등급을 반환하는 함수 `calculateGrade`를 작성하십시오.
```kotlin
fun calculateGrade(score: Int): String {
    return when (score) {
        in 90..100 -> "A"
        in 80 until 90 -> "B"
        in 70 until 80 -> "C"
        in 60 until 70 -> "D"
        else -> "F"
    }
}

fun main() {
    val score = 85
    val grade = calculateGrade(score)
    println("성적 등급: $grade")
}
```

---

## 34. 이넘 (Enum)
- 이넘은 서로 관련 있는 값들을 나타내는 데 사용되며, 코틀린에서는 enum 키워드를 사용하여 선언합니다.
- 이넘은 일반 클래스처럼 메서드나 프로퍼티를 가질 수 있습니다.

**문제:** 이넘을 활용하여 간단한 요일 열거형을 정의하고, 이를 사용하여 오늘의 요일을 출력하는 프로그램을 작성하십시오.
```kotlin
enum class DayOfWeek {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

fun main() {
    val today = DayOfWeek.SATURDAY
    println("오늘은 ${today.name}입니다.")
}
```

---

## 35. 데이터 클래스 (Data classes)
- 데이터 클래스는 데이터를 표현하기 위한 클래스로, 주로 데이터의 구조체를 나타내는 데 사용됩니다.
- 데이터 클래스는 toString(), equals(), hashCode() 등의 메서드를 자동으로 생성해줍니다.

**문제:** 데이터 클래스 Person을 선언하고, 다양한 사람 객체를 생성하고 출력하는 프로그램을 작성하십시오.

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val person1 = Person("홍길동", 30)
    val person2 = Person("아무개", 25)
    println("사람 1: $person1")
    println("사람 2: $person2")
}
```

---

## 36. 해체 선언 (Destructuring declaration)
- 해체 선언은 복합적인 데이터 구조에서 각각의 요소를 추출하여 변수에 할당하는 방법을 제공합니다.
- 주로 Pair, Triple 등의 데이터 클래스와 함께 사용됩니다.

**문제:** Pair 클래스를 사용하여 두 개의 숫자를 입력받고, 해체 선언을 사용하여 두 숫자를 더하는 함수 `addNumbers`를 작성하십시오.
```kotlin
fun addNumbers(pair: Pair<Int, Int>): Int {
    val (num1, num2) = pair
    return num1 + num2
}

fun main() {
    val numbers = Pair(3, 5)
    val sum = addNumbers(numbers)
    println("두 숫자의 합: $sum")
}
```

---

## 37. 널이 될 수 있는 타입 (Types that can be null)
- 널이 될 수 있는 타입은 변수에 null을 할당할 수 있는 타입을 의미합니다.
- 널이 될 수 있는 타입을 다룰 때에는 안전한 접근 방식이 필요합니다.

**문제:** 널이 될 수 있는 정수형 변수를 다루는 함수 `printNullable`을 작성하십시오. 변수가 널이 아닌 경우 값을 출력하고, 널인 경우 "널 값입니다"를 출력하십시오.
```kotlin
fun printNullable(value: Int?) {
    if (value != null) {
        println("값: $value")
    } else {
        println("널 값입니다")
    }
}

fun main() {
    val number1: Int? = 42
    val number2: Int? = null
    printNullable(number1)
    printNullable(number2)
}
```

---

## 38. 안전 호출과 엘비스 연산자 (Safe calls and Elvis operators)
- 안전 호출 연산자(?.)와 엘비스 연산자(?:)는 널이 될 수 있는 타입을 다룰 때 유용한 연산자입니다.
- 안전 호출 연산자는 null 체크를 간편하게 수행하고, 엘비스 연산자는 널 값 대체 기능을 제공합니다.

**문제:** 널이 될 수 있는 문자열을 안전 호출과 엘비스 연산자를 사용하여 안전하게 처리하는 함수 printLength를 작성하십시오.
```kotlin
fun printLength(text: String?) {
    val length = text?.length ?: 0
    println("문자열 길이: $length")
}

fun main() {
    val str1: String? = "Hello"
    val str2: String? = null
    printLength(str1)
    printLength(str2)
}
```

---

## 39. 단언문 (Assertion that it is not you)
- 단언문(assert)은 프로그램 실행 중에 조건을 검사하고, 조건이 충족되지 않으면 예외를 발생시키는 역할을 합니다.
- 단언문을 사용하여 프로그램의 불변 조건을 검증할 수 있습니다.

**문제:** 단언문(assert)을 사용하여 사용자의 입력을 검사하고, 입력 값이 특정 조건을 충족하지 않으면 예외를 발생시키는 함수 `checkInput`을 작성하십시오.
```kotlin
fun checkInput(input: Int) {
    assert(input > 0) { "입력값은 양수여야 합니다." }
    println("입력값은 유효합니다: $input")
}

fun main() {
    val value1 = 42
    val value2 = -5

    checkInput(value1) // 유효한 입력
    checkInput(value2) // 예외 발생
}
```

---

## 40. 확장 함수와 널 타입 (Extension functions and nullable types)
- 널 타입과 확장 함수를 함께 사용하면 널 값을 가진 객체에도 확장 함수를 적용할 수 있습니다.
- 이를 통해 널 값 처리를 간편하게 수행할 수 있습니다.

**문제:** 널 타입과 확장 함수를 함께 사용하여 널 값을 가진 문자열에 대해 대문자로 변환하는 함수 `uppercaseOrNull`을 작성하십시오.
```kotlin
fun String?.uppercaseOrNull(): String? {
    return this?.toUpperCase()
}

fun main() {
    val text1: String? = "hello"
    val text2: String? = null

    val result1 = text1.uppercaseOrNull()
    val result2 = text2.uppercaseOrNull()

    println("Result 1: $result1")
    println("Result 2: $result2")
}
```

---

## 41. 제네릭 소개 (Introduction to Generics)
- 제네릭은 타입을 파라미터화하여 코드의 재사용성을 높이는 기법입니다.
- 제네릭을 사용하면 다양한 타입의 데이터를 처리하는 함수나 클래스를 작성할 수 있습니다.

**문제:** 제네릭을 활용하여 다양한 데이터 타입을 다루는 함수 `printList`를 작성하십시오. 함수는 리스트의 요소를 출력해야 합니다.
```kotlin
fun <T> printList(list: List<T>) {
    for (item in list) {
        println(item)
    }
}

fun main() {
    val numbers = listOf(1, 2, 3, 4, 5)
    val names = listOf("Alice", "Bob", "Charlie")

    println("Numbers:")
    printList(numbers)

    println("Names:")
    printList(names)
}
```

---

## 42. 확장된 프로퍼티 (Extended properties)
- 확장된 프로퍼티는 기존 클래스에 새로운 프로퍼티를 추가하는 방법을 제공합니다.
- 이를 통해 기존 클래스의 구조를 확장할 수 있습니다.

**문제:** 확장된 프로퍼티를 사용하여 사각형의 너비와 높이를 저장하고, 넓이를 계산하는 프로그램을 작성하십시오.

```kotlin
class Rectangle(var width: Int, var height: Int)

val Rectangle.area: Int
    get() = width * height

fun main() {
    val rectangle = Rectangle(5, 10)
    val area = rectangle.area
    println("사각형의 넓이: $area")
}
```

---

## 43. break와 continue (break and continue)
- break와 continue는 반복문에서 제어 흐름을 조작하는데 사용됩니다.
- break는 반복문을 종료하고, continue는 현재 반복을 건너뛰고 다음 반복으로 진행합니다.

**문제:** 반복문을 사용하여 1부터 100까지의 숫자 중에서 3으로 나누어 떨어지는 숫자를 출력하되, 10개까지만 출력하는 프로그램을 작성하십시오.
```kotlin
fun main() {
    var count = 0
    for (number in 1..100) {
        if (number % 3 == 0) {
            println(number)
            count++
        }
        if (count >= 10) {
            break
        }
    }
}
```

---

## 44. 레이블 (Label)
- 레이블은 반복문이나 표현식에 이름을 부여하는 기능을 제공합니다.
- 레이블을 사용하여 중첩 반복문에서 특정 반복문을 선택적으로 제어할 수 있습니다.

**문제:** 중첩된 반복문을 사용하여 2차원 배열의 특정 요소를 검색하고 해당 요소를 출력하는 프로그램을 작성하십시오. 레이블을 활용하여 특정 반복문을 선택적으로 제어하십시오.
```kotlin
fun main() {
    val matrix = arrayOf(
        intArrayOf(1, 2, 3),
        intArrayOf(4, 5, 6),
        intArrayOf(7, 8, 9)
    )

    val target = 5
    var found = false

    search@ for (row in matrix) {
        for (element in row) {
            if (element == target) {
                println("요소를 찾았습니다: $element")
                found = true
                break@search
            }
        }
    }

    if (!found) {
        println("요소를 찾지 못했습니다.")
    }
}
```

---


