#### chapter4. 기초 연습문제

### 연습문제 1: 숫자 필터링

정수 목록이 주어지면 람다 식을 사용하여 목록에서 모든 짝수를 필터링하고,

홀수만 포함된 새 목록을 반환하는 고차 함수를 작성합니다.

```kotlin
fun filterOddNumbers(numbers: List<Int>): List<Int> {
    return numbers.filter { it % 2 != 0 }
}
// Example usage:
// val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9)
// val oddNumbers = filterOddNumbers(numbers) println(oddNumbers)

// Output: [1, 3, 5, 7, 9]
```

---


### 연습문제 2: 이름 매핑

이름 목록을 만듭니다(예: "Alice", "Bob", "Charlie")

이 목록을 사용하고 람다 식을 사용하여 각 이름을 대문자 형식으로 변환하는 새 목록을 반환하는 고차 함수를 작성하세요.

```kotlin
fun transformToUppercase(names: List<String>): List<String> {
    return names.map { it.uppercase() }
}
// Example usage:
// val names = listOf("Alice", "Bob", "Charlie")
// val uppercaseNames = transformToUppercase(names) println(uppercaseNames)

// Output: [ALICE, BOB, CHARLIE]
```

---


### 연습문제 3: 합계 계산
목록에 있는 숫자의 제곱의 합을 계산하는 고차 함수를 작성하세요.

람다 표현식을 사용하여 각 숫자를 제곱한 다음 reduce와 같은 고차 함수를 사용하여 합계를 계산합니다.

```kotlin
fun sumOfSquares(numbers: List<Int>): Int {
    return numbers.map { it * it }.reduce { acc, num -> acc + num }
}
// Example usage:
// val numbers = listOf(1, 2, 3, 4, 5)
// val sum = sumOfSquares(numbers) println(sum)

// Output: 55
```

---


### 연습문제 4: 단어 수

문장이 문자열로 주어지면 문장에서 각 단어의 발생 횟수를 세고 키가 단어이고 값이 해당 개수인 맵에 결과를 저장하는 함수를 작성합니다.

문장을 단어로 분할하려면 람다 식을 사용하세요.

```kotlin
fun countWords(sentence: String): Map<String, Int> {
    val words = sentence.split(Regex("\\s+"))
    return words.groupingBy { it }.eachCount()
}
// Example usage:
// val sentence = "This is a sample sentence. This sentence is a sample."
// val wordCounts = countWords(sentence) println(wordCounts)

// Output: {This=2, is=2, a=2, sample=2, sentence.=2, sentence=2}
```

---


### 연습문제 5: 지역 함수

로컬 함수를 사용하여 양의 정수의 계승을 계산하는 함수를 작성하세요.

로컬 함수는 두 개의 매개변수, 즉 현재 결과와 곱할 현재 숫자를 사용해야 합니다.

로컬 함수 내에서 재귀를 사용하여 계승을 계산합니다.

```kotlin
fun computeFactorial(n: Int): Long {
    fun factorialHelper(current: Int, accumulator: Long): Long {
        return if (current == 0) {
        accumulator
        } else {
        factorialHelper(current - 1, current.toLong() * accumulator)
        }
    }
    return factorialHelper(n, 1)
}
// Example usage:
// val n = 5 val factorial = computeFactorial(n)
// println("Factorial of $n is $factorial")

// Output: Factorial of 5 is 120
```

---


### 연습문제 6: 길이별 필터링

주어진 최소 길이를 기준으로 문자열 목록을 필터링하는 고차 함수를 작성합니다.

함수는 문자열 목록과 최소 길이를 나타내는 정수를 받아야 합니다.

지정된 최소값보다 길거나 같은 문자열만 포함하는 새 목록을 반환해야 합니다.

```kotlin
fun filterByLength(strings: List<String>, minLength: Int): List<String> {
    return strings.filter { it.length >= minLength }
}
// Example usage:
// val strings = listOf("apple", "banana", "cherry", "date")
// val filteredStrings = filterByLength(strings, 6)
// println(filteredStrings)

// Output: [banana, cherry]
```

---


### 연습문제 7: 길이별 그룹화

문자열 목록이 주어지면 문자열을 길이별로 그룹화하여 맵으로 그룹화하는 함수를 작성합니다.

맵의 키는 문자열의 길이여야 하며 값은 해당 길이의 문자열 목록이어야 합니다.

```kotlin
fun groupByLength(strings: List<String>): Map<Int, List<String>> {
    return strings.groupBy { it.length }
}
// Example usage:
// val strings = listOf("apple", "banana", "cherry", "date")
// val groupedByLength = groupByLength(strings) println(groupedByLength)

// Output: {5=[apple, date], 6=[banana, cherry]} 
```


---

