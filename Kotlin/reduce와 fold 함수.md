코틀린에는 컬렉션에 있는 요소를 누적 연산하여 반환하는 reduce, fold 함수가 있습니다.

## 함수 뜯어보기
### reduce
```kotlin
public inline fun <S, T : S> Iterable<T>.reduce(
    operation: (S, T) -> S
): S
```
`reduce`의 초기값은 컬렉션의 첫번째 요소고, 반환 값은 컬렉션의 자료형이 되는 것을 알 수 있습니다.

### fold
```kotlin
public inline fun <T, R> Iterable<T>.fold(
    initial: R,
    operation: (R, T) -> R
): R
```

`fold`의 초기값은 파라미터를 통해 자유롭게 정할 수 있으며, 반환 값은 초기값의 자료형이 되는 것을 알 수 있습니다.

## 작성 방법

### 1부터 10까지 합 구하기
1부터 10까지의 정수가 담긴 List의 누적합을 구할때, 다음과 같이 사용합니다. `reduce`는 리스트의 첫번째 요소를 초기값으로 사용하고, `fold`는 argument로 초기값을 지정해주어야 합니다.

```kotlin
fun main(args: Array<String>) {
    val numbers = (1..10).toList()

    // total의 초기값은 numbers[1]로 시작한다.
    val sumUsingReduce = numbers.reduce {total, num ->
        total + num
        }
    println(sumUsingReduce) // 55

    // total의 초기값은 100으로 시작한다.
    val sumUsingFold = numbers.fold(100) { total, num ->
        total + num
        }
    println(sumUsingFold) // 155
}

```

### 첫 번째 요소에 따른 예외사항
위 코드를 보면 reduce와 fold는 상당히 유사해 보입니다. 그러나, 다음과 같은 코드에서 `reduce`를 이용하면 의도한 값과는 다른값이 반환됩니다.

다음 코드는 1 부터 5까지의 각 요소에 2를 곱한 값을 누적하여 반환합니다.
```kotlin
fun main(args: Array<String>) {
    val numbers = (1..5).toList()

    // total의 초기값은 numbers[1]로 시작한다.
    val sumUsingReduce = numbers.reduce {total, num ->
        total + num * 2
        }
    println(sumUsingReduce) // 29(?)

    // total의 초기값은 0으로 시작한다.
    val sumUsingFold = numbers.fold(0) { total, num ->
        total + num * 2
        }
    println(sumUsingFold) // 30
}
```
의도한 값은 30입니다. 그러나 `reduce`를 사용하면 29라는 잘못된 값이 나옵니다. 그 이유는 `reduce`는 다음 코드와 같이 초기값을 첫번째 요소로 정해놓고 **다음 요소부터 연산을 시작**하기 때문입니다.
```kotlin
fun reduce(numbers: List<Int>): Int{
    var total = numbers[0] // 첫번째 요소는 초기값으로 들어간다.
    for (i in 1 until numbers.size) {
        val num = numbers[i] // 그 다음 요소부터 연산 시작
        total += num * 2
    }
    return total
}
```
따라서 첫 번째 요소도 연산에 포함시키고 싶을 경우, `fold`를 사용하셔야합니다.

### Empty 리스트에 따른 반환값
앞서 설명했듯, `reduce`는 반환 자료형은 리스트의 자료형을 반환합니다. 그에 반면, `fold`는 **argument로 전달한 초기값의 자료형**을 반환합니다.

그렇다면 **비어있는 리스트**에 대해서는 어떤 값을 반환할까요? 다음 코드와 같이 `reduce`를 사용해봅시다.
```kotlin
    val numbers = intArrayOf()

    // total의 초기값은 numbers[1]로 시작한다.
    val sumUsingReduce = numbers.reduce {total, num ->
        total + num
        }
    println(sumUsingReduce)
```
코드를 입력하고 실행해보면, 런타임에서 다음과 같은 오류메세지를 띄웁니다.
```kotlin
Exception in thread "main" java.lang.UnsupportedOperationException: Empty array can't be reduced.
	at MainKt.main(Main.kt:21)
```

그 이유는 간단합니다. 초기값을 `total`에 할당하고 연산을 시작해야 하는데 할당할 값이 없다는거죠.

그렇다면 초기값을 할당한 `fold`에서는 결과가 나올까요?
```kotlin
fun main(args: Array<String>) {
    val numbers = intArrayOf()

    // total의 초기값은 0으로 시작한다.
    val sumUsingFold = numbers.fold(11) { total, num ->
        total + num
        }
    println(sumUsingFold)
}
<Console>
11
```
코드를 입력하고 실행하면 런타임 오류 없이 초기값이 출력되는 것을 보실 수 있습니다.

## 마치며
>이번 포스팅에서는 collection을 누적하여 계산해주는 `reduce`, `fold` 함수에 대해 알아보았는데요, `fold` 함수는 런타임 에러에도 안전하고, 더 세밀한 계산이 가능했습니다. 그에 반해 `reduce` 함수는 런타임에러에 안전하지 못하고 다소 불안정해 보입니다. `reduce`만의 장점을 아시는 분들은 댓글로 남겨주시기 바랍니다. 

## Reference
[Kotlin] reduce 와 fold: https://blog.leocat.kr/notes/2020/03/09/kotlin-reduce-and-fold
