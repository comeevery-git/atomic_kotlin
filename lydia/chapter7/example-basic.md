#### chapter7. 기초 연습문제(확장 람다, 제네릭)

### 연습문제 1: 확장 람다

String 에 확장 람다를 적용해보세요.

```kotlin
// 확장 함수 + 람다 적용
val printLength: String.() -> Unit = {
    println("문자열 길이: $length")
}

fun main() {
    val myString = "Hello, Kotlin!"
    myString.printLength() // "문자열 길이: 13" 출력
}
```
```kotlin
// 일반 함수 예시
fun printLength(str: String) {
    println("문자열 길이: ${str.length}")
}

fun main() {
    val myString = "Hello, Kotlin!"
    printLength(myString) // "문자열 길이: 13" 출력
}
```
```kotlin
// 일반적인 기본 코드만 적용한 예시
fun main() {
    val myString = "Hello, Kotlin!"
    println("문자열 길이: ${myString.length}") // "문자열 길이: 13" 출력
}
```


---


### 연습문제 2: 확장 람다

목록에서 두 번째로 높은 숫자를 반환하는 List<Int> 클래스에 대한 확장 함수를 만듭니다.

목록에 고유 숫자가 2개 미만인 경우 'null'을 반환합니다.

- secondHighest는 List<Int>의 확장 함수입니다. 모든 List<Int> 객체에서 호출될 수 있음을 의미합니다.
- 함수 내에서 목록을 SortedSet로 변환하여 중복을 제거하고 요소를 정렬합니다.
- 세트에 요소가 2개 미만인 경우 'null'을 반환합니다(두 번째로 높은 숫자가 존재하지 않음을 나타냄)
- 그렇지 않으면 정렬된 세트에서 마지막에서 두 번째 요소(원래 목록에서 두 번째로 높은 숫자)를 반환합니다.

```kotlin
fun List<Int>.secondHighest(): Int? {
    val sortedSet = this.toSortedSet()
    return if (sortedSet.size < 2) null else sortedSet.elementAt(sortedSet.size - 2)
}

fun main() {
    val numbers = listOf(3, 5, 7, 5, 2, 9, 9)
    val secondHighest = numbers.secondHighest()
    println("The second-highest number is: $secondHighest") // Should print 7
}
```


---


### 연습문제 3: 제네릭

제네릭 사용과 in 변성, out 변성 에너테이션을 사용하여 문제를 해결합니다.

`ItemProvider`, `ItemCollector`, `Transporter` 클래스를 사용하여 특정 아이템을 수집하고 배포하는 프로그램을 완성하세요.

in 변성은 아이템을 수집하는 데 사용되며, out 변성은 아이템을 배포하는 데 사용됩니다.

```kotlin
/**
 * 문제지
 */
// TODO: 아이템을 배포하는 인터페이스 작성 ItemProvider

// TODO: 아이템을 수집하는 인터페이스 작성 ItemCollector

class Transporter<T>(private val provider: ItemProvider<T>, private val collector: ItemCollector<T>) {
    fun transportItem() {
        val item = provider.getItem()
        collector.collectItem(item)
        println("아이템이 전송되었습니다: $item")
    }
}

data class Item(val name: String) // 샘플 아이템 클래스

fun main() {
    val itemProvider = object : ItemProvider<Item> {
        override fun getItem(): Item = Item("샘플 아이템")
    }

    val itemCollector = object : ItemCollector<Item> {
        override fun collectItem(item: Item) {
            println("아이템 수집됨: $item")
        }
    }

    val transporter = Transporter(itemProvider, itemCollector)
    transporter.transportItem()
}
```

---

```kotlin
/**
 * 답안지
 */
// 아이템을 제공하는 인터페이스
interface ItemProvider<out T> {
    fun getItem(): T
}

// 아이템을 수집하는 인터페이스
interface ItemCollector<in T> {
    fun collectItem(item: T)
}

// 트랜스포터 클래스
class Transporter<T>(private val provider: ItemProvider<T>, private val collector: ItemCollector<T>) {
    fun transportItem() {
        val item = provider.getItem()
        collector.collectItem(item)
        println("아이템이 전송되었습니다: $item")
    }
}

// 샘플 아이템 클래스
data class Item(val name: String)

fun main() {
    val itemProvider = object : ItemProvider<Item> {
        override fun getItem(): Item = Item("샘플 아이템")
    }

    val itemCollector = object : ItemCollector<Item> {
        override fun collectItem(item: Item) {
            println("아이템 수집됨: $item")
        }
    }

    val transporter = Transporter(itemProvider, itemCollector)
    transporter.transportItem()
}
```


---

