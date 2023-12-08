#### 챕터6. 실패 방지하기

[Exceptions | Kotlin] (https://kotlinlang.org/docs/exceptions.html#the-nothing-type)


## 73. 예외 처리
- 오류 보고
  - 코틀린의 오류 보고 체계는 자바와 비슷하다.
  - 코틀린은 Checked Exception 을 제공하지 않는다.
    - kotlin은 왜 Java와 달리 Checked Exception을 제공하지 않을까?
      - https://kth990303.tistory.com/393
- 복구
  - 예외를 던지면 예외를 잡아서 복구할 수 있다.
  - try 블록은 예외가 발생할 수 있는 코드를 포함한다.
  - catch 블록은 예외를 잡아서 처리하는 코드를 포함한다.
  - finally 블록은 예외가 발생하든 발생하지 않든 반드시 실행되는 코드를 포함한다.
```kotlin
fun main() {
  val reader = BufferedReader(FileReader("input.txt"))
  try {
    println(reader.readText())
  } catch (e: IOException) {
    println("읽을 수 없습니다.")
  } finally {
    reader.close()
  }
}
```

---

> 연습문제 - ExceptionHandling/Task2.kt
```
- findNumber(s: String)는 s를 검색하고 숫자가 포함된 문자열을 반환합니다.
- 숫자가 없으면 NoNumber가 발생합니다.
- ConvertNumber(s: String)은 s를 Int로 변환합니다.
- s를 Int로 변환할 수 없으면 BadNumber가 발생합니다.
- embedNumber(n: Int)는 일부 문자 내에 n을 포함하는 문자열을 생성합니다.

문제는 두 가지 기능을 정의하는 것입니다.
1. justFail(s: String)은 위의 세 가지 함수를 호출하여 호출 내에 호출을 중첩합니다.
   String 내에서 숫자를 찾아 Int로 변환하고 해당 Int를 String 내에 삽입한 다음 결과와 함께 Trace()를 호출합니다.
2. Recover(s: String)는 위의 각 함수를 한 번에 하나씩 호출하여 각 호출의 실패를 복구하여 다음 호출이 성공할 수 있도록 합니다. findNumber()가 실패하면 복구는 문자열 "0"을 생성합니다. ConvertNumber()가 실패하면 복구는 -1을 생성합니다. Recover()가 끝나면 결과와 함께 Trace()를 호출합니다.
```
```kotlin
// AtomicKotlin StudyPlugin 에서 진행
// ExceptionHandling/Task2.kt
package exceptionHandlingExercise2
import atomictest.trace

open class NumberFail : Exception()
open class NoNumber : NumberFail()
open class BadNumber : NumberFail()

fun findNumber(s: String): String {
  var result = ""
  for (c in s)
    if (c in "0123456789.-")
      result += c
    else if (result.isNotEmpty())
      return result
  throw NoNumber()
}

fun convertNumber(s: String): Int =
  try {
    s.toInt()
  } catch (e: NumberFormatException) {
    throw BadNumber()
  }

fun embedNumber(n: Int) = "AbCdE${n}fGhIj"

fun justFail(s: String) =
  "TODO"

fun recover(s: String) {
  TODO()
}

fun test(s: String) {
  trace("justFail($s)")
  justFail(s)
  trace("recover($s)")
  recover(s)
}

fun main() {
  test("The13thFloor9")
  test("NoDigitsHere")
  test("negative-11int")
  test("A float: 3.14159 (pi)")
  trace eq """
    justFail(The13thFloor9)
    AbCdE13fGhIj
    recover(The13thFloor9)
    AbCdE13fGhIj
    justFail(NoDigitsHere)
    exceptionHandlingExercise2.NoNumber
    recover(NoDigitsHere)
    AbCdE0fGhIj
    justFail(negative-11int)
    AbCdE-11fGhIj
    recover(negative-11int)
    AbCdE-11fGhIj
    justFail(A float: 3.14159 (pi))
    exceptionHandlingExercise2.BadNumber
    recover(A float: 3.14159 (pi))
    AbCdE-1fGhIj
  """
}
```


---


## 74. 검사 명령
- require()
- requireNotNull()
- check()
- assert()
```kotlin
fun checkVariable(value: Int?) {
  requireNotNull(value) { "Value must not be null." }
  require(value >= 0) { "Value must be non-negative." }
  check(value < 100) { "Value must be less than 100." }
  assert(value % 2 == 0) { "Value must be even." }
}
```


---


## 75. Nothing 타입
- Any, Unit, Nothing
  - 함수가 리턴 될 일이 없을 경우
  - 예외를 던지는(throw Exception) 함수의 리턴 타입
  - https://readystory.tistory.com/143

```kotlin
fun main() {
  val x = null // Nothing?
  val l = listOf(null) // List<Nothing?>
  
  println(x is Nothing?) // true
  println(l is List<Nothing?>) // true
}
```


---



