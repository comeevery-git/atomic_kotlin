#### chapter2. 기초 연습문제

## 테스트 (Tests)

- 간단한 함수 테스트하기
```kotlin
fun double(x: Int): Int {
    return x * 2
}

// Test 코드 예시 (JUnit 사용)
@Test
fun testDouble() {
    assertEquals(4, double(2))
}
```

- 예외 처리 테스트하기
```kotlin
fun checkPositive(x: Int) {
    if (x <= 0) throw IllegalArgumentException("Number must be positive")
}
```

- Mock 객체 사용하기 - Spring boot
```kotlin
@Service
class MyService(private val myRepository: MyRepository) {
    fun findData(id: Long): MyData {
        return myRepository.findById(id)
    }
}

@Repository
interface MyRepository {
    fun findById(id: Long): MyData
}

data class MyData(val id: Long, val name: String)
```

```kotlin
@SpringBootTest
class MyServiceTest {
    @MockBean
    private lateinit var myRepository: MyRepository

    @Autowired
    private lateinit var myService: MyService

    @Test
    fun testFindData() {
        // 준비 (Arrange)
        val myData = MyData(1L, "Test Data")
        // 메소드가 호출될 때 특정 값을 반환하도록 모의 설정
        Mockito.`when`(myRepository.findById(1L)).thenReturn(myData) 
        // 실행 (Act)
        val result = myService.findData(1L)

        // 검증 (Assert)
        assertEquals(myData, result)
    }
}
```

---

## 예외 (Exceptions)

- 사용자 정의 예외 만들기
```kotlin
class MyException(message: String) : Exception(message)

fun myFunction(x: Int) {
    if (x < 0) throw MyException("Negative value not allowed")
}
```

- 예외 처리하기/예외 전파하기
```kotlin
try {
    myFunction(-1)
} catch (e: MyException) {
    println(e.message)
}
```

---

## 가변 인자 목록 (Variable Argument Lists)

- 가변 인자를 받는 함수 작성하기
```kotlin
fun printVarargs(vararg strings: String) {
    strings.forEach { println(it) }
}
```

- 가변 인자와 일반 인자를 함께 사용하기
```kotlin
fun printNumbers(number: Int, vararg numbers: Int) {
    println(number)
    numbers.forEach { println(it) }
}
```

- 가변 인자를 이용한 리스트 생성하기
```kotlin
fun <T> listOfVarargs(vararg elements: T): List<T> {
    return elements.toList()
}
```

---

## 집합 (Sets)

- 집합 생성 및 요소 추가하기
```kotlin
val mySet = mutableSetOf(1, 2, 3)
mySet.add(4)
```

- 집합의 교집합, 합집합, 차집합 구하기
```kotlin
val setA = setOf(1, 2, 3)
val setB = setOf(3, 4, 5)
val union = setA.union(setB) // 합집합
val intersection = setA.intersect(setB) // 교집합
val difference = setA.subtract(setB) // 차집합
```

- 집합 순회하기
```kotlin
val mySet = setOf(1, 2, 3)
mySet.forEach { println(it) }
```

---

## 맵 (Maps)

- 맵의 키-값 쌍 추가 및 제거하기
```kotlin
val myMap = mutableMapOf("a" to 1, "b" to 2)
myMap["c"] = 3 // 추가
myMap.remove("a") // 제거
```

- 맵의 값에 접근하고 수정하기
```kotlin
val value = myMap["b"]
myMap["b"] = 20
```

- 맵에서 조건에 맞는 요소 필터링하기
```kotlin
val filteredMap = myMap.filter { it.value > 1 }
```

---

## 속성 접근자 (Property Accessors)

- 커스텀 게터 만들기
```kotlin
class MyClass {
    var x: Int = 0
    val isPositive: Boolean
        get() = x > 0
}
```

- 커스텀 세터 만들기
```kotlin
class MyClass {
    var y: Int = 0
        set(value) {
            field = if (value > 0) value else 0
        }
}
```

- 백킹 필드를 사용하는 속성 만들기
```kotlin
class MyClass {
    var z: Int = 0
        get() = field
        set(value) {
            field = value.coerceAtLeast(0)
        }
}
```

---