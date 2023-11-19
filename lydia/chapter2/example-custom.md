#### chapter2. 커스텀 연습문제


## 배열

### 나누어 떨어지는 숫자 배열
- https://school.programmers.co.kr/learn/courses/30/lessons/12910
  - 정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다. (중복이 없다.)
  - 아무것도 나누어 떨어지지 않는다면 배열에 -1을 담아 반환해야한다.



## list

### 리스트 만들기
- 숫자를 두 개 입력받은 후, 두 번째 숫자를 첫 번째 숫자만큼 가지고 있는 리스트를 반환하는 함수를 만들어보자.
  - 예를 들어 replicate(3, 5)를 입력하면 5가 3개 있는 리스트 [5, 5, 5]를 반환한다.
> 함수의 선언 타입은 다음과 같습니다.
>
> `fun replicate(n:Int, element: Int): List<Int>`
```kotlin
fun replicate(n:Int, element:Int): List<Int> {
  return when(n) {
    1 -> listOf(element)
    else -> listOf(element)+replicate(n-1, element)
  }
}
```
- https://zerodice0.tistory.com/206

### 여러가지 타입의 값 넣기
- 리스트에 정해진 자료형만 들어가는 것은 아닙니다.
  - 제네릭을 선언해주지 않으면 다양한 자료형을 리스트에 담을 수 있습니다.
```kotlin
fun main(args: Array<String>) {
    var mixedType = listOf("hello", 123, true, 123.456)
    for (m in mixedType) print("$m ")
      println()
}
```

### 빈 리스트 생성
- emptyList 로도 생성해줄 수 있습니다.
```kotlin
fun main(args: Array<String>) {
    var emptyList = emptyList<String>()
    val emptyList: List<Int> = emptyList()
}
```

### null이 아닌 element만 리스트에 담기
- 어떤 리스트에 null이 들어있다면 이 null을 필터링해서 null이 아닌 값만 담아줄 수 있는 리스트가 있는데, 바로 listOfNotNull입니다.
```kotlin
fun main(args: Array<String>) {
  val notNullList:List<Int> = listOfNotNull(2, 3, 4, null, 5, 7, null)    //이라면
  println(notNullList)    // null은 반영되지 않음
  println(notNullList[3])
}
```

### 불변형 리스트를 가변형 리스트로 바꿔주기
```kotlin
fun main(args: Array<String>) {
    // 가변형 리스트를
    var names:List<String> = listOf<String>("one", "two", "three")

    // 불변형 리스트로
    val newNames = names.toMutableList()
    newNames.add("five")
    println(newNames)
}
```

### 클래스 작성하기
```kotlin
class Duck(val name:String, val age:Int)
// 이 클래스를 리스트의 제네릭으로 지정해주고 값을 넣어보겠습니다.

fun main(args: Array<String>) {
    val list:MutableList<Duck> = mutableListOf()

    val duck = Duck("오리1", 2)
    list.add(duck)
    list.add(Duck("오리2", 2))
    list.add(Duck("오리3", 4))

    for (d in list) {
        println(d.name)
    }
}
```

### map 사용하기
```kotlin
fun main() {
  val a: List<Int> = listOf(1, 2, 3)
  val b = a.map { it * 10 } // it 키워드로 각 요소 값 접근 가능
  println(b) // [10, 20, 30]

  val dataList = listOf(MyData(1, "hello"), MyData(2, "world"))
  dataList.map { (a, b) -> println("$a, $b") }  // 각 데이터 클래스 인스턴스를 구조 분해
}
```
- 위 예제는 IntRange 클래스에 속하는 두 객체(인스턴스)를 만든다.
  - 각 객체는 메모리에 자신만의 저장공간을 가지고 있다.
  - IntRange 는 클래스지만, 0부터 10까지의 범위를 가리키는 r1 범위는 r2 와 구분되는 별도의 객체다.
    map() Collection 에 사용할 수 있는 고차 함수이다.
  - Collection 을 구성하는 각 요소들에 대해 특정 표현식에 의거하여 변형을 거친 뒤 새로운 Collection 을 반환해준다.


***


- 공식 홈페이지 참고
  - https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-map/


***


