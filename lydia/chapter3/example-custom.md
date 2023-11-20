#### chapter3. 커스텀 연습문제

## Data class

### 문제 1. 직원 관리 시스템
- 여러분은 회사의 직원 관리 시스템을 개발하고 있습니다. 이 시스템은 직원의 정보를 관리하며, 직원의 정보를 비교, 복사 및 출력하는 기능이 필요합니다.

- Data Class 정의
  - Employee라는 데이터 클래스를 정의합니다. 이 클래스는 name (문자열), id (정수), department (문자열)을 속성으로 가집니다.
- equals()와 hashCode() 사용
  - 두 Employee 객체가 동일한지 비교하는 코드를 작성합니다. 또한, hashCode()를 사용하여 두 객체가 동일한 해시코드를 가지는지 확인합니다.
- toString() 사용
  - Employee 객체를 문자열로 변환하여 출력하는 코드를 작성합니다.
- copy() 사용
  - Employee 객체를 복사하여 새로운 객체를 만들되, 일부 속성을 변경하는 코드를 작성합니다.
- 구조 분해 선언 사용
  - Employee 객체로부터 name, id, department를 구조 분해 선언을 사용하여 추출하는 코드를 작성합니다.

```kotlin
data class Employee(val name: String, val id: Int, val department: String)

fun main() {
  val employee1 = Employee("John Doe", 1001, "Finance")
  val employee2 = Employee("John Doe", 1001, "Finance")
  val employee3 = employee1.copy(department = "HR")

  println(employee1.toString())
  println("Are employee1 and employee2 equal? ${employee1 == employee2}")
  println("Hashcode of employee1: ${employee1.hashCode()}, employee2: ${employee2.hashCode()}")

  val (name, id, department) = employee3
  println("Name: $name, ID: $id, Department: $department")
}
```

### 문제 2. 도서 목록 관리
- 여러분은 도서관이나 서점의 도서 목록 관리 시스템을 개발하고 있습니다. 이 시스템은 도서의 정보를 저장하고, 도서 목록에서 특정 조건을 만족하는 도서를 찾아내야 합니다.

- Data Class 정의
  - Book이라는 데이터 클래스를 정의합니다. 이 클래스는 title (문자열), author (문자열), year (정수)를 속성으로 가집니다.
- 리스트에서 copy()와 구조 분해 선언 사용
  - 여러 Book 객체를 포함하는 리스트를 생성합니다. 리스트의 각 객체를 복사하여 속성을 변경하고, 구조 분해 선언을 사용하여 각 속성을 추출하는 코드를 작성합니다.
- 두 Book 객체가 동일한지 비교하는 코드를 작성합니다.

```kotlin
data class Book(val title: String, val author: String, val year: Int)

fun main() {
  val books = listOf(
    Book("Kotlin Programming", "John Doe", 2018),
    Book("Effective Java", "Joshua Bloch", 2008)
  )

  val updatedBooks = books.map { it.copy(year = 2021) }
  updatedBooks.forEach { (title, author, year) ->
    println("Title: $title, Author: $author, Year: $year")
  }

  val book1 = Book("Kotlin Programming", "John Doe", 2018)
  val book2 = Book("Kotlin Programming", "John Doe", 2018)
  println("Are book1 and book2 equal? ${book1 == book2}")
}
```


---


## 안전호출, 엘비스 연산자, Data Class, When 

### 문제 1. 학생 관리 시스템
- Student라는 data class를 만드세요.
  - 이 클래스는 name (문자열), grade (정수), 그리고 email (nullable 문자열)을 프로퍼티로 가져야 합니다.
- Student 객체의 리스트를 만들고, 이 중에서 email이 null이 아닌 학생들의 이름과 이메일을 출력하는 함수를 작성하세요.
  - email이 null인 경우, "Email not provided"라고 출력하세요.
- 각 학년별로 "Welcome to [학년] grade, [이름]!" 메시지를 출력하는 함수를 when 표현식을 사용하여 작성하세요.

```kotlin
data class Student(val name: String, val grade: Int, val email: String?)

fun printStudentEmails(students: List<Student>) {
  students.forEach { student ->
    val emailMessage = student.email ?: "Email not provided"
    println("${student.name}: $emailMessage")
  }
}

fun greetStudent(student: Student) {
  val message = when (student.grade) {
    10 -> "Welcome to 10th grade, ${student.name}!"
    11 -> "Welcome to 11th grade, ${student.name}!"
      else -> "Hello, ${student.name}!"
  }
    println(message)
}
```


### 문제 2. 상품 가격 계산기
- Product라는 data class를 만드세요.
  - 이 클래스는 name (문자열), price (실수), 그리고 discount (nullable 실수)를 프로퍼티로 가져야 합니다.
- 할인된 가격을 계산하는 함수를 작성하세요.
  - 이 함수는 Product 객체를 받아, 할인이 있으면 할인된 가격을, 없으면 원래 가격을 반환합니다.
  - 할인율은 0과 1 사이의 값으로, 가격에 곱해집니다.
- 상품의 종류에 따라 세금을 계산하는 함수를 when 표현식을 사용하여 작성하세요.
  - 예를 들어, "food"의 경우 세금은 0.05(5%), "clothing"의 경우 0.1(10%) 등으로 설정하세요.

```kotlin
data class Product(val name: String, val price: Double, val discount: Double?)
  fun calculateDiscountedPrice(product: Product): Double {
    val discountMultiplier = product.discount ?: 1.0
    return product.price * discountMultiplier
  }
  
  fun calculateTax(product: Product): Double {
    val taxRate = when (product.name) {
        "food" -> 0.05
        "clothing" -> 0.1
    else -> 0.08
  }
  return product.price * taxRate
}
```


---


## 확장 함수, 오버로딩, 구조 분해 선언, Enum class

### 문제 1. 커스텀 문자열 처리
- String 클래스에 대한 확장 함수를 작성하세요.
  - 이 함수는 문자열에서 모든 공백을 제거하고, 모든 문자를 대문자로 변환합니다.
- 두 개의 String 파라미터를 받는 오버로딩된 함수를 작성하세요.
  - 첫 번째 문자열에서 모든 공백을 제거한 후, 두 번째 문자열을 연결합니다.
- Pair 클래스를 사용하여 두 문자열을 담은 후, 구조 분해 선언을 사용하여 두 문자열을 분리하고 결과를 출력하는 코드를 작성하세요.

```kotlin
fun String.removeSpacesAndUpperCase(): String {
  return this.replace(" ", "").uppercase()
}

fun String.concatWithoutSpaces(other: String): String {
    return this.replace(" ", "") + other
}

fun main() {
  val str = "Hello World"
  println(str.removeSpacesAndUpperCase())

  val combined = "Hello".concatWithoutSpaces("World")
  println(combined)

  val pair = Pair("First", "Second")
  val (first, second) = pair
  println("$first, $second")
}
```


### 문제 2. 간단한 Enum 클래스와 사용
- Color라는 Enum 클래스를 생성하세요.
  - 이 클래스는 적어도 RED, GREEN, BLUE 색상을 갖고 있어야 합니다.
- Color Enum 클래스에 확장 함수를 추가하여, 각 색상에 대한 대응하는 RGB 값을 반환하게 하세요.
- 각 색상의 RGB 값을 출력하는 코드를 작성하세요.
  - 여기서 구조 분해 선언을 사용하여 RGB 각 값에 접근하세요.

```kotlin
enum class Color {
  RED, GREEN, BLUE;
}

fun Color.toRGB(): Triple<Int, Int, Int> {
  return when(this) {
    Color.RED -> Triple(255, 0, 0)
    Color.GREEN -> Triple(0, 255, 0)
    Color.BLUE -> Triple(0, 0, 255)
    }
}

fun main() {
  val redRGB = Color.RED.toRGB()
  val (r, g, b) = redRGB
  println("Red: R=$r, G=$g, B=$b")
}
```


---


## 복합적인 연습문제

### 문제 1. 특별 이벤트 참가자 관리 시스템
- 여러분은 특별 이벤트에 참가하는 사람들을 관리하는 시스템을 구축하고 있습니다. 이 시스템은 참가자의 정보를 관리하고, 참가자의 이벤트 참가 상태에 따라 특별한 메시지를 출력해야 합니다.

- Data Class 정의
  - Participant라는 데이터 클래스를 정의합니다. 이 클래스는 name (문자열), email (nullable 문자열), 그리고 status (Enum 클래스 ParticipationStatus의 인스턴스)를 속성으로 가집니다.
- Enum Class 정의
  - ParticipationStatus라는 Enum 클래스를 정의합니다. 이 클래스는 REGISTERED, WAITING, ATTENDED를 상태로 가집니다.
- 확장 함수 작성
  - ParticipationStatus에 대한 확장 함수 statusMessage를 작성합니다. 이 함수는 각 상태에 대해 적절한 메시지를 반환합니다.
- 엘비스 연산자 사용
  - Participant의 email이 null인 경우, "이메일 없음"으로 처리하는 로직을 작성합니다.
- when 사용
  - Participant의 status에 따라 다른 처리를 하는 로직을 when을 사용하여 작성합니다.
- 구조 분해 선언 사용
  - Participant 객체로부터 name과 email을 구조 분해 선언을 사용하여 추출합니다.
- 오버로딩된 함수 작성
  - Participant의 이름을 출력하는 함수를 작성합니다. 이 함수는 Participant 객체와 String (접두사) 두 가지 형태의 파라미터를 받을 수 있도록 오버로딩합니다.

```kotlin
data class Participant(val name: String, val email: String?, val status: ParticipationStatus)

enum class ParticipationStatus {
  REGISTERED, WAITING, ATTENDED
}

fun ParticipationStatus.statusMessage() = when(this) {
  ParticipationStatus.REGISTERED -> "등록 완료"
  ParticipationStatus.WAITING -> "대기 중"
  ParticipationStatus.ATTENDED -> "참석함"
}

fun printName(participant: Participant) {
  println(participant.name)
}

fun printName(prefix: String, participant: Participant) {
  println("$prefix ${participant.name}")
}

fun main() {
  val participant = Participant("John Doe", null, ParticipationStatus.REGISTERED)
  val (name, email) = participant

  val emailMessage = email ?: "이메일 없음"
  println("이름: $name, 이메일: $emailMessage, 상태: ${participant.status.statusMessage()}")

  printName(participant)
  printName("참가자:", participant)
}
```


### 문제 2. 도서관 관리 시스템
- 여러분은 도서관의 도서 관리 시스템을 개발하고 있습니다. 이 시스템은 다양한 종류의 도서를 관리하며, 도서의 상태에 따라 다른 처리를 해야 합니다.

- Data Class 정의
  - Book이라는 데이터 클래스를 정의합니다. 이 클래스는 title (문자열), author (문자열), yearPublished (null이 될 수 있는 정수), 그리고 status (Enum 클래스 BookStatus의 인스턴스)를 속성으로 가집니다.
- Enum Class 정의
  - BookStatus라는 Enum 클래스를 정의합니다. 이 클래스는 AVAILABLE, BORROWED, IN_REPAIR, LOST를 상태로 가집니다.
- 확장 함수 작성
  - BookStatus에 대한 확장 함수 description을 작성합니다. 이 함수는 각 상태에 대해 적절한 설명을 반환합니다.
- 엘비스 연산자 사용
  - Book의 yearPublished가 null인 경우, "출판 연도 미상"으로 처리하는 로직을 작성합니다.
- when 사용
  - Book의 status에 따라 다른 처리를 하는 로직을 when을 사용하여 작성합니다.
- 구조 분해 선언 사용
  - Book 객체로부터 title과 author를 구조 분해 선언을 사용하여 추출합니다.
- 제네릭 함수 작성
  - 여러 종류의 Book 객체들을 받아, 특정 조건에 맞는 책들만을 리스트로 반환하는 제네릭 함수를 작성합니다.
- 오버로딩된 함수 작성
  - Book의 정보를 출력하는 함수를 작성합니다. 이 함수는 Book 객체와 Boolean (상세 정보 출력 여부) 두 가지 형태의 파라미터를 받을 수 있도록 오버로딩합니다.
- Loop와 Break/Continue 사용
  - 주어진 Book 리스트에서 특정 조건을 만족하는 첫 번째 책을 찾아 출력하는 로직을 작성합니다. break와 continue를 적절히 사용하세요.

```kotlin
data class Book(val title: String, val author: String, val yearPublished: Int?, val status: BookStatus)

enum class BookStatus {
  AVAILABLE, BORROWED, IN_REPAIR, LOST
}

fun BookStatus.description() = when(this) {
  BookStatus.AVAILABLE -> "사용 가능"
  BookStatus.BORROWED -> "대출 중"
  BookStatus.IN_REPAIR -> "수리 중"
  BookStatus.LOST -> "분실"
}

fun printBookInfo(book: Book, detailed: Boolean = false) {
  println("제목: ${book.title}, 저자: ${book.author}")
  if (detailed) {
    val year = book.yearPublished ?: "출판 연도 미상"
    println("출판 연도: $year, 상태: ${book.status.description()}")
  }
}

fun <T : Book> findBooks(books: List<T>, condition: (T) -> Boolean): List<T> {
  return books.filter(condition)
}

fun main() {
  val books = listOf(
    Book("Kotlin Programming", "John Doe", 2018, BookStatus.AVAILABLE),
    Book("The Joy of Kotlin", "John Smith", null, BookStatus.BORROWED)
  )

  val (title, author) = books[0]
  println("첫 번째 책: $title, 저자: $author")

  for (book in books) {
    if (book.status == BookStatus.LOST) continue
    if (book.status == BookStatus.AVAILABLE) {
      printBookInfo(book)
      break
    }
  }

  val availableBooks = findBooks(books) { it.status == BookStatus.AVAILABLE }
  println("사용 가능한 책: ${availableBooks.map { it.title }}")
}
```


***


- 공식 홈페이지 참고
  - https://kotlinlang.org/docs/generics.html
  - https://kotlinlang.org/docs/data-classes.html


***


