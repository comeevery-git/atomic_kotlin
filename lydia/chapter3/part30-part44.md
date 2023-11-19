#### 챕터3. 사용성

## 30. 확장 함수 (Extension functions)
```kotlin
fun List<Int>.multiplyAll() = this.reduce { acc, num -> acc * num }
```

---

## 31. 이름 붙은 인자와 기본 인자 (Named and default arguments)
```kotlin
fun printUserInfo(name: String, age: Int = 30, city: String = "서울") {
    println("이름: $name, 나이: $age, 도시: $city")
}
```

## 32. 오버로딩 (Overloading)
```kotlin
fun add(a: Int, b: Int) = a + b
fun add(a: Double, b: Double) = a + b
```

## 33. when 표현식 (when expression)
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
```

## 34. 이넘 (Enum)
```kotlin
enum class DayOfWeek {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

## 35. 데이터 클래스 (Data classes)
```kotlin
    data class Person(val name: String, val age: Int)
```

## 36. 해체 선언 (Destructuring declaration)
```kotlin
fun addNumbers(pair: Pair<Int, Int>): Int {
    val (num1, num2) = pair
    return num1 + num2
}
```

---

## 37. 널이 될 수 있는 타입 (Types that can be null)
```kotlin
fun printNullable(value: Int?) {
    if (value != null) {
        println("값: $value")
    } else {
        println("널 값입니다")
    }
}
```

---

## 38. 안전 호출과 엘비스 연산자 (Safe calls and Elvis operators)
```kotlin
fun printLength(text: String?) {
    val length = text?.length ?: 0
    println("문자열 길이: $length")
}
```

---

## 39. 단언 (Assertion)
```kotlin
fun checkInput(input: Int) {
    assert(input > 0) { "입력값은 양수여야 합니다." }
    println("입력값은 유효합니다: $input")
}
```

---

## 40. 확장 함수와 널 타입 (Extension functions and nullable types)
```kotlin
fun String?.uppercaseOrNull(): String? {
    return this?.toUpperCase()
}
```

---

## 41. 제네릭 소개 (Introduction to Generics)
```kotlin
fun <T> printList(list: List<T>) {
    for (item in list) {
        println(item)
    }
}
```

---

## 42. 확장된 프로퍼티 (Extended properties)
```kotlin
class Rectangle(var width: Int, var height: Int)

val Rectangle.area: Int
    get() = width * height
```

---

## 43. break와 continue (break and continue)
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


