#### chapter6. 기초 연습문제

### 연습문제 1: 예외 처리

두 개의 정수를 입력으로 받아 나누기를 수행하는 'divideAndReport' 함수를 작성하세요.

0으로 나누기 예외를 처리하고 그러한 예외가 발생하면 오류 메시지를 보고합니다.

```kotlin
fun divideAndReport(a: Int, b: Int) {
    try {
        val result = a / b
        println("Result of division: $result")
    } catch (e: ArithmeticException) {
        println("Error: Division by zero is not allowed.")
    }
}

fun main() {
    divideAndReport(10, 2) // Result of division: 5
    divideAndReport(5, 0)  // Error: Division by zero is not allowed.
}
```


---


### 연습문제 2: 검사 순서

다양한 검사 기능(require, requireNotNull, check 및 assert)을 사용하여 변수가 유효한지 확인하는 함수가 있습니다.

이러한 함수를 사용하는 올바른 순서를 보여주는 checkVariable 함수를 작성하세요.

```kotlin
fun checkVariable(value: Int?) {
requireNotNull(value) { "Value must not be null." }
require(value >= 0) { "Value must be non-negative." }
check(value < 100) { "Value must be less than 100." }
assert(value % 2 == 0) { "Value must be even." }
}

fun main() {
    checkVariable(42)      // No exceptions
    checkVariable(null)    // IllegalArgumentException: Value must not be null.
    checkVariable(-5)      // IllegalArgumentException: Value must be non-negative.
    checkVariable(150)     // IllegalStateException: Value must be less than 100.
    checkVariable(7)       // AssertionError: Value must be even.
}
```


---


### 연습문제 3: Nothing Type

의도적으로 값을 반환하지 않지만 Nothing 유형을 사용하여

여전히 유효한 Kotlin 함수인 neverReturns 함수를 작성하세요.

```kotlin
fun neverReturns(): Nothing {
    throw IllegalStateException("This function intentionally returns no value.")
}

fun main() {
    val result = neverReturns() // 컴파일 오류. Variable 'result' must be initialized
}
```


---

