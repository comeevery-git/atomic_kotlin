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


