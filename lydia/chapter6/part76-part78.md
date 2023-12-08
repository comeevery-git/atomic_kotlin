#### 챕터6. 실패 방지하기


## 76. 자원 해제
- 자원 해제
  - 자바의 try-with-resources 구문과 비슷한 코틀린의 use() 함수를 사용한다.
  - use() 함수는 AutoCloseable 인터페이스를 구현하는 클래스에 대해 사용할 수 있다.
  - use() 함수는 예외가 발생하더라도 자원을 해제한다.
  - Java, Kotlin, Python에서의 자원의 할당과 해제
    - try/catch, runCatching, use
    - https://velog.io/@kshired/Java-Kotlin-Python%EC%97%90%EC%84%9C%EC%9D%98-%EC%9E%90%EC%9B%90%EC%9D%98-%ED%95%A0%EB%8B%B9%EA%B3%BC-%ED%95%B4%EC%A0%9C
```kotlin
fun main() {
  // use() 함수를 사용하여 자원을 해제한다.
  val reader = BufferedReader(FileReader("input.txt"))
  reader.use {
    println(it.readText())
  }
}
```


---


## 77. 로깅
- trace, debug, info, warn, error, fatal
  - https://www.baeldung.com/kotlin-logging
- trace() 함수는 가장 낮은 로깅 수준, fatal() 함수는 가장 높은 로깅 수준이다.
- slf4j 라이브러리를 사용하여 로깅을 수행한다.
  - https://www.slf4j.org/
  - https://mvnrepository.com/artifact/org.slf4j/slf4j-api
```kotlin
fun main() {
  // trace
  logger.trace("Hello, Kotlin Logging!")
  // debug
  logger.debug("Hello, Kotlin Logging!")
  // 로깅 수준을 지정하지 않으면 info() 함수가 기본 로깅 수준이다.
  logger.info("Hello, Kotlin Logging!")
}
```


---


## 78. 단위 테스트
- 단위(Unit) 테스트는 프로그램의 작은 부분을 테스트하는 것이다.
- 단위 테스트를 작성하는 방법
  - https://kotlinlang.org/api/latest/kotlin.test/
  - https://kotlinlang.org/docs/jvm-test-using-junit.html#add-dependencies
  - 우아한형제들 기술블로그
    - https://techblog.woowahan.com/5825/
  - JUnit 5
    - https://junit.org/junit5/docs/current/user-guide/
```kotlin
// junit5 테스트 코드
import org.junit.jupiter.api.Assertions.assertEquals
import org.junit.jupiter.api.Test

class CalculatorTest {
  @Test
  fun testAdd() {
    val calculator = Calculator()
    assertEquals(30, calculator.add(10, 20))
  }
}
```


---

