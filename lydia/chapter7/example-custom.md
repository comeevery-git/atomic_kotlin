#### chapter7. 커스텀 연습문제(연산자 오버로딩, 프로퍼티 위임, 지연 계산 초기화, 늦은 초기화)


---


### 연습문제 1: 학생 점수 계산기

사용자가 학생의 이름과 과목 점수 목록을 입력할 수 있는 클래스 StudentGrade를 작성하세요.
이 클래스는 학생의 이름 및 점수를 저장하며, 점수는 몇 가지 과목에 대한 평균 및 합계를 계산하는 기능을 제공해야 합니다.

또한, + 연산자 오버로딩을 사용하여 두 개의 StudentGrade 인스턴스를 합칠 수 있어야 합니다.
합쳐진 결과는 이름은 그대로 유지하고 각 과목의 점수는 더해져야 합니다.

```kotlin
// 연산자 오버로딩, 클래스 생성, 평균 및 합계 계산 사용
class StudentGrade(val name: String, val scores: List<Int>) {
// TODO: + 연산자 오버로딩을 구현하세요. (학생 점수 합산)

    // TODO: 과목 점수의 평균 계산을 위한 함수를 구현하세요.

    // TODO: 과목 점수의 합계 계산을 위한 함수를 구현하세요.
}

// Example usage:
val student1 = StudentGrade("Alice", listOf(85, 90, 78))
val student2 = StudentGrade("Bob", listOf(75, 88, 92))

val combinedStudent = student1 + student2 // 두 학생의 점수 합산
val average = combinedStudent.calculateAverage() // 평균 계산
val total = combinedStudent.calculateTotal() // 합계 계산
println("이름: ${combinedStudent.name}, 평균: $average, 합계: $total")
```


---


### 연습문제 2: 상품 주문 계산기

주문한 상품 목록을 표현하는 클래스 Order를 작성하세요.
Order 클래스는 상품 이름과 각 상품의 가격 목록을 저장하며, 주문한 총 가격을 계산할 수 있어야 합니다.

또한, 상품의 가격 목록을 변경할 때 Order 클래스에서 사용자 정의된 위임 속성을 사용하여 변경 내용을 기록해야 합니다.
변경 내용은 수정 전 가격과 수정 후 가격을 저장하는 형태로 기록됩니다.

마지막으로, 주문한 상품 목록을 처음 액세스할 때만 실제로 상품 정보를 로드하는 lazy 속성을 사용해야 합니다.

```kotlin
// 위임 속성, lazy 초기화 사용
class Order {
    // 사용자 정의 위임 속성을 사용하여 상품 가격 변경 기록
    private var _itemPrices: Map<String, Double> by PriceChangeLogging(mapOf())

    // TODO: 주문한 상품 목록을 처음 액세스할 때만 실제로 상품 정보를 로드하도록 구현하세요. (lazy 초기화)
    // 구현할 메서드 이름: loadOrderedItems

    // TODO: 주문한 총 가격을 계산하는 함수를 구현하세요.
    // 구현할 메서드 이름: calculateTotalPrice

    // 상품 정보 로드 시뮬레이션
    private fun loadOrderedItems(): List<OrderedItem> {
        return listOf(
            OrderedItem("ProductA", 2),
            OrderedItem("ProductB", 3),
            OrderedItem("ProductC", 1)
        )
    }
}

data class OrderedItem(val name: String, val quantity: Int)

// 사용자 정의 위임 속성을 사용하여 가격 변경 내역 기록
class PriceChangeLogging(initialPrices: Map<String, Double>) {
    private var prices: Map<String, Double> = initialPrices

    operator fun getValue(thisRef: Any?, property: KProperty<*>): Map<String, Double> {
        return prices
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: Map<String, Double>) {
        val oldPrices = prices
        prices = value
        logPriceChanges(oldPrices, value)
    }

    private fun logPriceChanges(oldPrices: Map<String, Double>, newPrices: Map<String, Double>) {
        for ((product, oldPrice) in oldPrices) {
            val newPrice = newPrices[product]
            if (oldPrice != newPrice) {
                println("가격 변경: $product, 이전 가격: $oldPrice, 새로운 가격: $newPrice")
            }
        }
    }
}

// Example usage:
val order = Order()
order.orderedItems // 처음 액세스 시 상품 정보 로드됨
val totalPrice = order.calculateTotalPrice()
println("주문 총 가격: $totalPrice")
```


---


### 연습문제 1: 학생 점수 계산기 `답안지`
```kotlin
// 연산자 오버로딩, 클래스 생성, 평균 및 합계 계산 사용
class StudentGrade(val name: String, val scores: List<Int>) {
    // + 연산자 오버로딩을 구현 (학생 점수 합산)
    operator fun plus(other: StudentGrade): StudentGrade {
        // 이름은 그대로 유지하고 각 과목 점수 합산
        val combinedScores = (this.scores zip other.scores) { a, b -> a + b }
        return StudentGrade(this.name, combinedScores)
    }

    // 과목 점수의 평균 계산 함수 구현 (평균 계산)
    fun calculateAverage(): Double {
        return scores.average()
    }

    // 과목 점수의 합계 계산 함수 구현 (합계 계산)
    fun calculateTotal(): Int {
        return scores.sum()
    }
}

// Example usage:
val student1 = StudentGrade("Alice", listOf(85, 90, 78))
val student2 = StudentGrade("Bob", listOf(75, 88, 92))

val combinedStudent = student1 + student2 // 두 학생의 점수 합산
val average = combinedStudent.calculateAverage() // 평균 계산
val total = combinedStudent.calculateTotal() // 합계 계산
println("이름: ${combinedStudent.name}, 평균: $average, 합계: $total")
```


---


### 연습문제 2: 상품 주문 계산기 `답안지`
```kotlin
// 위임 속성, lazy 초기화 사용
class Order {
    // 사용자 정의 위임 속성을 사용하여 상품 가격 변경 기록
    private var _itemPrices: Map<String, Double> by PriceChangeLogging(mapOf())

    // 주문한 상품 목록을 처음 액세스할 때만 실제로 상품 정보를 로드하도록 구현 (lazy 초기화)
    // 구현할 메서드 이름: loadOrderedItems
    val orderedItems by lazy { loadOrderedItems() }

    // 주문한 총 가격을 계산하는 함수 구현
    // 구현할 메서드 이름: calculateTotalPrice
    fun calculateTotalPrice(): Double {
        return orderedItems.sumByDouble { item -> _itemPrices.getValue(item.name) * item.quantity }
    }

    // 상품 정보 로드 시뮬레이션
    private fun loadOrderedItems(): List<OrderedItem> {
        return listOf(
            OrderedItem("ProductA", 2),
            OrderedItem("ProductB", 3),
            OrderedItem("ProductC", 1)
        )
    }
}

data class OrderedItem(val name: String, val quantity: Int)

// 사용자 정의 위임 속성을 사용하여 가격 변경 내역 기록
class PriceChangeLogging(initialPrices: Map<String, Double>) {
    private var prices: Map<String, Double> = initialPrices

    operator fun getValue(thisRef: Any?, property: KProperty<*>): Map<String, Double> {
        return prices
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: Map<String, Double>) {
        val oldPrices = prices
        prices = value
        logPriceChanges(oldPrices, value)
    }

    private fun logPriceChanges(oldPrices: Map<String, Double>, newPrices: Map<String, Double>) {
        for ((product, oldPrice) in oldPrices) {
            val newPrice = newPrices[product]
            if (oldPrice != newPrice) {
                println("가격 변경: $product, 이전 가격: $oldPrice, 새로운 가격: $newPrice")
            }
        }
    }
}

// Example usage:
val order = Order()
val totalPrice = order.calculateTotalPrice()
println("주문 총 가격: $totalPrice")
```


---

