#### 챕터7. 파워 툴

> 컴퓨터가 이해할 수 있는 코드를 작성하는 건 바보도 할 수 있다.
> 
> 좋은 프로그래머는 사람이 이해할 수 있는 코드를 작성한다. 
>
> - 마틴 파울러(Martin Fowler)

---


## 79. 확장 람다
- 확장 람다는 확장 함수와 비슷하다. 차이가 있다면 함수가 아니라 람다라는 점이다.
- 다음 코드에서 va와 vb는 같은 결과를 내놓는다.
```kotlin
val va: (String, Int) -> String = {str, n -> // 일반 람다
  str.repeat(n) + str.repeat(n)
}
val vb: String.(Int) -> String = { // String 파라미터를 괄호 밖으로 옮겨서 String.(Int)처럼 확장 함수 구문 사용
  this.repeat(it) + repeat(it) // this를 통해 수신 객체에 접근
}
  
fun main() {
  va("Vanbo", 2) eq "VanboVanboVanboVanbo"
  "Vanbo".vb(2) eq "VanboVanboVanboVanbo"
  vb("Vanbo", 2) eq "VanboVanboVanboVanbo"
  // "Vanbo".va(2) // Compile error
}
```
- 코틀린 문서에서는 확장 람다를 `수신 객체가 지정된 함수 리터럴` 이라고 말한다.
  - `함수 리터럴` 이라는 말은 람다와 익명 함수를 모두 포함한다.
  - 수신 객체가 `암시적 파라미터로 추가 지정된 람다`라는 사실을 강조하고 싶을 때는
    - 수신 객체가 지정된 람다를 `확장 람다`의 동의어로 더 자주 사용한다.
- 확장 함수와 마찬가지로 확장 람다도 여러 파라미터를 받을 수 있다.

- 참고링크
  - [코틀린 공식 문서 - 수신 객체가 지정된 함수 리터럴](https://kotlinlang.org/docs/lambdas.html#function-literals-with-receiver)
  - [코틀린 공식 문서 - 확장 람다](https://kotlinlang.org/docs/lambdas.html#extension-lambdas)
  - [코틀린 공식 문서 - 확장 함수](https://kotlinlang.org/docs/extensions.html#extension-functions)
  - [확장 함수, 람다 함수, 고차 함수의 기초](https://mashup-android.vercel.app/mashup-11th/Ahnseokjoo/baselambda/BaseLambda)

---


## 80. 영역 함수
- 영역 함수는 인라인된다
```kotlin
fun main() {

}
```


---


## 81. 제네릭스 만들기
- Anu
- 제네릭스 정의하기
- 타입 정보 보존하기
- 타입 파라미터 제약
- 타입 소거
- 함수의 타입 인자에 대한 실체화
- 타입 변성
```kotlin
fun main() {

}
```


---

