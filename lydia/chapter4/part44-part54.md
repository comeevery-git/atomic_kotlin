#### 챕터4. 함수형 프로그래밍


## 44. 람다
- 람다를 사용하면 이해하기 쉬운 간결한 코드를 작성할 수 있다.
- 람다는 부가적인 장식이 덜 들어간 함수다(람다를 함수 리터럴)이라고 부르기도 한다.
  - 함수 리터럴은 함수를 값으로 취급할 수 있게 한다.
  - 람다식은 익명 함수를 만들기 위한 식으로, 함수의 이름이 없고, 매개변수와 함수의 본문으로 이루어져 있다.
```kotlin
{ x: Int, y: Int -> x + y }

val sum = { x: Int, y: Int -> x + y }

val sum: (Int, Int) -> Int = { x, y -> x + y }

val sum = { x: Int, y: Int ->
  println("Computing the sum of $x and $y...")
  x + y
}
```

- 아래 예제에서는 람다를 List<Int>에 사용 중이므로 코틀린은 n의 타입이 Int라는 사실을 알 수 있다.
```kotlin
val list = listOf(1, 2, 3, 4, 5)
val result = list.map({ n -> "[$n]" })
result eq listOf("[1]", "[2]", "[3]", "[4]", "[5]")
```

- 파라미터가 하나일 경우 코틀린은 자동으로 파라미터 이름을 it 으로 만든다. 이 말은 람다의 파라미터가 하나일 경우 it 을 사용할 수 있다는 뜻이다.
- map은 모든 타입의 List에 사용될 수 있다. 다음 코드에서 코틀린은 람다의 it이 Char 타입이라는 사실을 추론한다.
- 함수의 파라미터가 람다뿐이면 람다 주변의 괄호를 없앨 수 있으므로, 더 깔끔하게 코드를 적을 수 있다.
```kotlin
val list = listOf('a', 'ab', 'b')
val result = list.map { "[{$it.toUpperCase()}]" }
result eq listOf("[A]", "[AB]", "[B]")
```

- mapIndexed() 라이브러리 함수를 사용하면 람다의 파라미터로 원소의 인덱스를 추가할 수 있다.
- 람다가 특정 인자를 사용하지 않는 경우 밑줄을 사용할 수 있다. 밑줄을 쓰면 람다가 무슨 무슨 인자를 사용하지 않는다는 컴파일러 경고를 무시할 수 있다.
```kotlin
val list = listOf('a', 'b', 'c')
list.mapIndexed { index, element -> "$index: $element" } eq
  listOf("0: a", "1: b", "2: c")

val list = listOf('a', 'b', 'c')
list.indices.map {
    "[$it]"
} eq listOf ("[0]", "[1]", "[2]")
```


---


## 45. 람다의 중요성
- 람다는 문법 설탕처럼 보일 수 있다. 하지만 람다는 프로그래밍에 중요한 능력을 부여해준다.

- 아래 예제에서 even과 greaterThan2는 모두 filter()를 사용하며, 술어 부분만 다르다. 반복을 피하면서 간결하고 명확한 코드를 작성할 수 있게 해준다.
```kotlin
val list = listOf(1, 2, 3, 4, 5)
val even = list.filter { it % 2 == 0 }
val greaterThan2 = list.filter { it > 2 }
even eq listOf(2, 4)
greaterThan2 eq listOf(3, 4, 5)
```

- 람다는 다음과 같은 이유로 중요하다.
  - 람다는 함수를 값으로 취급할 수 있게 한다.
  - 람다는 함수를 인자로 받거나 반환할 수 있게 한다.
  - 람다는 함수를 간결하게 표현할 수 있게 한다.

- 아래 예제에서 람다는 자신의 밖에 정의된 val divider를 '포획'한다.
  - 람다는 포획한 요소를 읽을 수 있을 뿐 아니라 변경할 수도 있다.
```kotlin
val list = listOf(1, 5, 7, 10)
val divider = 5
list.filter {
    it % divider == 0 
} eq listOf(5, 10)
```

- forEach() 라이브러리 함수는 지정한 동작을 컬렉션의 매 원소에 적용한다.
```kotlin
val list = listOf(1, 5, 7, 10)
val divider = 5
list.filter { it % divider == 0 }
//  .forEach {sum += it }
    .sum() eq 15
```


---


## 46. 컬렉션에 대한 연산
- 함수형 프로그래밍의 능력들 중에서 객체 컬렉션에 대한 연산을 한꺼번에 수행할 수 있는 능력이 매우 중요하다.
- 컬렉션에 대한 연산은 다음과 같이 세 가지로 나눌 수 있다.
  - 컬렉션을 생성하는 연산
  - 컬렉션의 원소를 조작하는 연산
  - 컬렉션의 원소를 조건에 따라 선택하는 연산

```kotlin
fun main() {
  // 람다는 인자로 추가할 원소의 인덱스를 받는다
  val list1 = List(10) { it }
  list1 eq listOf(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
  
  // 한 값으로만 이뤄진 리스트
  val list2 = List(10) { 0 }
  list2 eq "[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]"
  
  // 글자로 이뤄진 리스트
  val list4 = List(10) { list3[it % 3]}
  list4 eq "[a, b, c, a, b, c, a, b, c, a]"
}
```

```kotlin
fun main() {
  val mutableList1 = MutableList(5, { 10 * (it + 1) })
  mutableList1 eq "[10, 20, 30, 40, 50]"
  
  val mutableList2 = MutableList(5) { 10 * (it + 1) }
  mutableList2 eq "[10, 20, 30, 40, 50]"
}
```
- List() 와 MutableList() 는 생성자가 아니라 함수다.
  - 두 함수의 이름을 지을 때 일부러 List라고 대문자로 시작해서 마치 생성자인 것처럼 보일 뿐이다.

- 다양한 컬렉션 함수가 술어를 받아서 컬렉션의 원소를 검사한다.
  - all() 함수는 컬렉션의 모든 원소가 술어를 만족하는지 검사한다.
  - any() 함수는 컬렉션의 원소 중 하나라도 술어를 만족하는지 검사한다.
  - none() 함수는 컬렉션의 모든 원소가 술어를 만족하지 않는지 검사한다.
  - count() 함수는 컬렉션의 원소 중 술어를 만족하는 원소의 개수를 반환한다.
  - find() 함수는 컬렉션의 원소 중 술어를 만족하는 첫 번째 원소를 반환한다.
  - first() 함수는 컬렉션의 첫 번째 원소를 반환한다.
  - last() 함수는 컬렉션의 마지막 원소를 반환한다.
  - firstOrNull() 함수는 컬렉션의 첫 번째 원소를 반환한다. 만약 컬렉션이 비어있다면 null을 반환한다.
  - lastOrNull() 함수는 컬렉션의 마지막 원소를 반환한다. 만약 컬렉션이 비어있다면 null을 반환한다.
  - max() 함수는 컬렉션의 원소 중 가장 큰 값을 반환한다.
  - min() 함수는 컬렉션의 원소 중 가장 작은 값을 반환한다.
  - filter()는 주어진 술어와 일치하는(술어가 true를 반환하는) 모든 원소가 들어 있는 새 리스트를 만든다.
    - 즉, 술어를 만족하는 원소만을 가진 컬렉션을 반환한다.
  - partition() 함수는 주어진 술어와 일치하는 원소와 일치하지 않는 원소를 각각 가진 쌍의 컬렉션을 반환한다.
    - 즉, 술어를 만족하는 원소와 만족하지 않는 원소를 각각 가진 컬렉션을 반환한다.


---


## 47. 멤버 참조
- 함수 인자로 멤버 참조를 전달할 수 있다.
- 함수, 프로퍼티, 생성자에 대해 만들 수 있는 멤버 참조는 해당 함수, 프로퍼티, 생성자를 호출하는 뻔한 람다를 대신할 수 있다.
  - 멤버 함수나 프로퍼티 이름 앞에 그들이 속한 클래스 이름과 2중 콜론(::)을 위치시켜서 멤버 참조를 만들 수 있다.
  - 다음 코드에서는 Message::isRead가 멤버 참조다.
```kotlin
data class Message(val sender: String, val content: String, val isRead: Boolean)

fun main() {
  val messages = listOf(
    Message("Kitty", "Hey!", true),
    Message("Kitty", "Where are you?", false)
  )
  val unread =
    messages.filterNot(Message::isRead)
  unread.size eq 1
  unread.single().text eq "Where are you?"
}
```
- filterNot(): 주어진 술어와 일치하지 않는 원소를 가진 컬렉션을 반환한다.

- 함수 참조
  - 함수 참조를 사용하면 람다를 별도의 함수로 추출할 수 있다.
    - 코드를 더 이해하기 쉽게 변경할 수 있다.
    - 코틀린에서는 함수 타입이 필요한 곳에 함수를 바로 넘길 수 없지만, 그 대신에 함수에 대한 참조는 넘길 수 있다.
- 생성자 참조
  - 클래스 이름을 사용해 생성자에 대한 참조를 만들 수도 있다.
    - 생성자 참조는 클래스 이름 뒤에 ::를 붙이면 된다.
- 확장 함수 참조
  - 확장 함수에 대한 참조를 만들 수도 있다.
    - 확장 함수 참조는 수신 객체 타입 이름을 함수 이름 앞에 덧붙인다.


---


## 48. 고차함수
- 프로그래밍 언어에서 함수를 다른 함수의 인자로 넘길 수 있거나 함수가 반환값으로 함수를 돌려줄 수 있으면,
  - 언어가 고차 함수(higher-order function)를 지원한다고 말한다.
- 고차 함수는 함수형 프로그래밍에서 필수적이다. 지금까지 다룬 filter(), map(), any() 등의 함수는 모두 고차 함수다.
- 코틀린에서는 함수를 일급 객체로 다룬다.
  - 즉, 함수를 일반 값처럼 다룰 수 있다.
  - 함수를 변수에 저장할 수 있고, 함수를 인자로 다른 함수에 전달할 수 있으며, 함수에서 새로운 함수를 만들어 반환할 수도 있다.
- 고차함수는 함수를 매개변수로 받거나 함수를 반환하는 함수를 말한다.
```kotlin
fun main() {
  val list = listOf(1, 2, 3, 4)
  println(list.filter { it % 2 == 0 })
  println(list.map { it * it })
  println(list.any { it % 2 == 0 })
  println(list.all { it % 2 == 0 })
  println(!list.all { it % 2 == 0 })
  println(list.find { it > 3 })
  println(list.count { it % 2 == 0 })
}
```


---


## 49. 리스트 조작하기
- 리스트 조작은 리스트의 원소를 조작하는 연산을 말한다.
- 묶기(zipping)와 평평하게 하기(flattening)는 리스트를 조작하는 두 가지 연산이다.
- 묶기는 두 개의 리스트를 하나의 리스트로 만드는 연산이다.
  - 두 리스트의 원소를 쌍으로 묶어서 새로운 리스트를 만든다.
  - 두 리스트의 원소 개수가 다르면 더 짧은 쪽의 원소 개수만큼만 새 리스트에 들어간다.
  - 코틀린에서는 zip() 함수를 사용해 두 리스트를 묶을 수 있다.
- 평평하게 하기는 리스트의 리스트를 하나의 리스트로 만드는 연산이다.
  - 코틀린에서는 flatten() 함수를 사용해 리스트의 리스트를 하나의 리스트로 만들 수 있다.
```kotlin
// zipping
val colors = listOf("red", "brown", "grey")
val animals = listOf("fox", "bear", "wolf")
colors zip animals eq listOf("red" to "fox", "brown" to "bear", "grey" to "wolf")

// flatten
val numbers = listOf(listOf(1, 2), listOf(3, 4))
numbers.flatten() eq listOf(1, 2, 3, 4)
```


---


## 50. 맵 만들기
- 맵은 키와 값의 쌍으로 이루어진 컬렉션이다.
- 코틀린에서는 mapOf() 함수를 사용해 맵을 만들 수 있다.
  - mapOf() 함수는 키와 값의 쌍을 인자로 받는다.
    - 키와 값의 쌍은 to라는 키워드로 만들 수 있다.

- groupBy() 함수는 리스트를 맵으로 변환한다.
  - groupBy() 함수는 키를 반환하는 함수를 인자로 받는다.
  - groupBy() 함수는 키를 반환하는 함수를 적용한 결과를 키로 하고, 원래의 원소를 값으로 하는 맵을 반환한다.
```kotlin
val numbers = listOf("one", "two", "three", "four")
numbers.groupBy { it.first().toUpperCase() } eq mapOf("O" to listOf("one", "two"), "T" to listOf("three", "four"))
```

- mapValues() 함수는 맵의 값을 변환한다.
  - mapValues() 함수는 변환 함수를 인자로 받는다.
  - mapValues() 함수는 맵의 각 키에 변환 함수를 적용한 결과를 값으로 하는 맵을 반환한다.
```kotlin
val numbers = mapOf(0 to "zero", 1 to "one")
numbers.mapValues { it.value.toUpperCase() } eq mapOf(0 to "ZERO", 1 to "ONE")
```

- associateBy()는 키를 반환하는 함수를 인자로 받는다.
  - associateBy() 함수는 키를 반환하는 함수를 적용한 결과를 키로 하고, 원래의 원소를 값으로 하는 맵을 반환한다.
```kotlin
val numbers = listOf("one", "two", "three", "four")
numbers.associateBy { it.first().toUpperCase() } eq mapOf("O" to "one", "T" to "three")
```

- associateWith()는 값을 반환하는 함수를 인자로 받는다.
  - associateWith() 함수는 원래의 원소를 키로 하고, 값을 반환하는 함수를 적용한 결과를 값으로 하는 맵을 반환한다.
```kotlin
val numbers = listOf("one", "two", "three", "four")
numbers.associateWith { it.length } eq mapOf("one" to 3, "two" to 3, "three" to 5, "four" to 4)
```

- getOrElse() 함수는 키에 대응하는 값이 있으면 그 값을 반환하고, 없으면 인자로 받은 함수를 호출해 값을 계산한다.
```kotlin
val numbers = mapOf("one" to 1, "two" to 2)
numbers.getOrElse("three") { 3 } eq 3
numbers.getOrElse("four") { 4 } eq 4
```

- getOrPut()는 MutableMap에만 적용할 수 있다.
  - getOrPut() 함수는 키에 대응하는 값이 있으면 그 값을 반환하고, 없으면 인자로 받은 함수를 호출해 값을 계산하고 맵에 추가한다.
```kotlin
val numbers = mutableMapOf("one" to 1, "two" to 2)
numbers.getOrPut("three") { 3 } eq 3
numbers.getOrPut("one") { 4 } eq 1
numbers eq mapOf("one" to 1, "two" to 2, "three" to 3)
```

- map을 다른 map으로 변환하는 연산을 맵 조작이라고 한다.
- 맵 조작은 맵의 키와 값을 변환하는 연산과 맵의 원소를 조작하는 연산으로 나눌 수 있다.

- map() 함수는 맵의 키와 값을 변환한다.
  - map() 함수는 변환 함수를 인자로 받는다.
  - map() 함수는 변환 함수를 적용한 결과를 키와 값의 쌍으로 하는 맵을 반환한다.
```kotlin
val numbers = mapOf(0 to "zero", 1 to "one")
numbers.mapValues { it.value.toUpperCase() } eq mapOf(0 to "ZERO", 1 to "ONE")
```

- mapKeys() 함수는 맵의 키를 변환한다.
  - mapKeys() 함수는 변환 함수를 인자로 받는다.
  - mapKeys() 함수는 변환 함수를 적용한 결과를 키로 하고, 원래의 원소를 값으로 하는 맵을 반환한다.
```kotlin
val numbers = mapOf("one" to 1, "two" to 2)
numbers.mapKeys { it.key.toUpperCase() } eq mapOf("ONE" to 1, "TWO" to 2)
```

- mapValues() 함수는 맵의 값을 변환한다.
  - mapValues() 함수는 변환 함수를 인자로 받는다.
  - mapValues() 함수는 변환 함수를 적용한 결과를 값으로 하고, 원래의 키를 키로 하는 맵을 반환한다.
```kotlin
val numbers = mapOf("one" to 1, "two" to 2)
numbers.mapValues { it.value + 1 } eq mapOf("one" to 2, "two" to 3)
```

- any()나 all() 함수를 사용해 맵에 특정 원소가 포함되어 있는지 검사할 수 있다.
```kotlin
val numbers = mapOf("one" to 1, "two" to 2)
numbers.any { it.value == 2 } eq true
numbers.all { it.value < 2 } eq false
```


---


