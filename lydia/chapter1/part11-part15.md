#### 챕터1. 프로그래밍 베이직

## 11. while로 반복하기
```kotlin
while(불리언 식) {
    // 반복할 코드
}
```
- 모든 산술 연산에 대해 더 짧은 연산자로 대입문을 수행할 수 있는 복합 대입 연산자
  - +=, -=, *=, /=, %=, ++, -- 지원
```kotlin
fun testCondition(i: Int) = i < 100
fun main() {
    var i = 0
    while (testCondition(i)) {
        print(".")
        i += 10
    }
}
// ..........
```
- while과 do 를 함께 쓸 수 있다.
```kotlin
do {
    // 반복할 코드
} while (불리언 식)
```

```kotlin
fun main() {
    var i = 0
    do {
        print(".")
        i += 10
    } while (i < 100)
}
// ..........
```


***


## 12. 루프와 범위
- 코틀린에서 for를 사용하면 범위나 String과 같이 이터레이션 가능한 객체로부터 원소를 직접 얻으면서 루프를 돌 수 있다.
```kotlin
fun main() {
    for (c in "Kotlin") { // String 은 Iterable 이다.
        print("$c ")
        // c += 1 // Error
        // val cannot be reassigned
    }
}
// k o t l i n
```
- c를 명시적으로 var 나 val 로 정의할 수는 없다.
  - 코틀린은 이 변수를 자동으로 val 로 정의하고 타입을 Char 로 추론한다.
- 범위를 사용하면 정숫값을 이터레이션 할 수 있다.
  - .. 으로 범위를 만들면 양 끝의 값을 포함한 범위가 생긴다.
  - 반면 until 로 범위를 만들면 오른쪽 끝 값을 제외한 범위가 생긴다.
    - 1 until 10 은 1..9 와 같다.
  - step 을 사용하면 1..21 step 3 처럼 증가값을 지정할 수 있다.
```kotlin
fun main() {
    for (i in 1..10) {
        print("$i ")
    }
}
// 1 2 3 4 5 6 7 8 9 10
```


***


## 13. in 키워드
- for 루프의 이터레이션에 쓴 것과 같은 in 키워드를 사용해 어떤 값이 범위에 속하는 지를 검사할 수 있다.
  - 반면 !in 은 대상 값이 범위 안에 속해있지 않은 경우 true 를 반환한다.
```kotlin
fun main() {
    val oneToTen = 1..10
    println(5 in oneToTen) // true
    println(11 in oneToTen) // false
    println(0 in oneToTen) // false
    println(-1 in oneToTen) // false
    println(5 !in oneToTen) // false
    println(11 !in oneToTen) // true
    println(0 !in oneToTen) // true
    println(-1 !in oneToTen) // true    
}
```


***


## 14. 식과 문
- 대부분의 프로그래밍 언어에서 가장 작은 유용한 코드 조각은 문 또는 식이다.
  - 문은 상태를 변경한다.
    - 결과값을 돌려주지 않는다.
      - 아무 값도 돌려주지 않기 때문에 뭔가 유용한 일을 하기 이ㅜ해서는 자신을 둘러싼 주변의 상태를 변경해야만 한다.
        - 이를 부수 효과 라고 한다.
  - 식은 값을 만든다.
    - 결과값을 돌려준다.
```kotlin
fun main() {
    val hours = 10 // 식
    val minutesPerHour = 60 // 식
    val minutes = hours * minutesPerHour // 식
}
```
- println() 같은 함수는 결과를 반환하지 않는 것처럼 보인다.
  - 하지만 println() 이라는 함수 호출도 여전히 식이기 때문에 무언가를 반드시 반환해야 한다.
  - 코틀린에서는 이런 목적으로 반환타입 Unit 을 사용한다.
```kotlin
fun main() {
  val result = println(42)
  println(result)
  // kotlin.Unit
}
```


***


## 15. 1부 요약


***