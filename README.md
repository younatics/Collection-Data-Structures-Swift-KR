# Collection Data Structures Swift - KR
Collection Data 스위프트 구조를 소개합니다.좋은 글이라 생각되어 번역합니다. 핵심적인 부분을 제외하고는 번역을 제외하고 의역했습니다. 본문은 [여기](https://www.raywenderlich.com/123100/collection-data-structures-swift-2)를 참고해주세요.

## Introduction
많은 데이터를 처리하는 어플리케이션을 만들고 있을 때를 생각해보세요. 언제 데이터를 넣고 어떻게 그 많은 데이터를 효율적으로 핸들링을 하실 건가요? 
많은 데이터를 처리 할 경우에는 `Array`, `Dictionarys`, `Sets` 에 대한 근본적인 개념을 알고 있는게 중요합니다. 이 `collection data structures` 에 명확한 개념을 가질 경우 처리 속도는 어마어마하게 빨라 질 것 입니다.
##### 듀토리얼의 순서는 다음과 같습니다.
1. 데이터 구조에 대해 개념을 가지고, `Big-O`표기법에 대해 배워봅니다.
2. 여러가지 자료 구조들에 대해 숙지합니다.
3. 데이터 구조들의 퍼포먼스를 비교해봅니다.
4. 추가의 팁으로 다른 구조들을 조금 더 알아봅니다.

## What is `Big-O` Notation?

Big-O 표현법은 정확하게 자료를 연산 할 때 시간 그래프를 표현 할 수 있는 방법입니다.
- `O(1)`: 제일 이상적인 퍼포먼스를 보여주는 그래프입니다. 연상의 횟수만큼 실행되는 경우입니다.
-  `O(log n)`: 좋은 퍼포먼스를 보여주는 그래프입니다. 데이터 구조에 있는 아이템만큼 점차적으로 연산의 횟수가 늘어나는 그래프입니다.
-  `O(n)`: 중간 단계의 퍼포먼스를 보여주는 그래프입니다. 데이터 구조랑 연산횟수가 일차함수를 그리며 증가합니다. 
- `O(n (log n))`: 연산횟수가 데이터 구조에 있는 아이템 * 데이터구조 아이템 수만큼 처리합니다. 낮은 단계의 퍼포먼스를 보여주는 그래프입니다.
- `O(n²) `: 데이터구조에 있느나이템수의 제곱만큼 늘어납니다. 여기서 부턴 성능이 매우 떨어지고, 추천하지 않습니다.
- `O(2^n)`: 2의 데이터 구조의 갯수만큼 증가합니다. 매우 나쁜 퍼포먼스를 보여줍니다.
- `O(n!)`: 최악의 케이스입니다. 이렇게 코딩이 될 경우에는 답이 없습니다.

(그래프 추가)

## Common iOS Data Structures
`Array`, `Dictionarys`, `Sets` 가 가장 흔한 데이터 구조입니다. 각각의 구조와 성능에 대해 알아봅니다. 또한 `Objectice-C`와`Swift`에서 각각의 데이터구조를 비교해봅니다.

###Arrays
- `Array`는 순서를 가지고 있는 배열입니다. `index`를 통해서 각각의 아이템에 접근 할 수 있습니다. 인덱스를 가지고 아이템을 가져오는 것을 `subscripting`이라고 합니다.
- `Objective-C`의 `NSArray`는 `Swift`에서 `let`으로 선언된 `Array`와 같습니다. `NSMutableArray`는 `var`로 선언된 `Array`와 같습니다.
- 최악의 케이스는 ` O(log n)`이 실행되지만 보통 `O(1)`이 연산됩니다.
- 모르는 오브젝트를 찾을 경우 최악은 `O(n (log n))`이 실행되지만 보통 `O(n)`만큼 연산됩니다.
- 데이터를 삭제하거나 넣는 경우 최악은 `O(n (log n))`이 걸리지만 보통 `O(1)`만큼 실행됩니다.

##### 이럴때 쓰면 좋습니다.
- `item`이 어디있는지 아는 경우 동작은 빠르게 작동 할 것입니다.
- 아이템이 어디있는지 모를 경우 처음부터 끝까지 동작해야하기 때문에 검색결과가 느립니다.
- 아이템을 어디에 넣고 삭제해야 하는지 알고 있다면 어렵지 않겠지만, 배열을 앞뒤로 살펴야 한다는 것은 시간 소비가 큽니다.

>`Swift`는 2015년 12월부터 오픈소스입니다. [Swift Code](https://swift.org/source-code/)로 들어가셔서 구조가 어떻게 되어있는 지 확인해보시는 것도 좋은 방법입니다.

아래는 `Objective-C`와 `Swift`의 비교 결과입니다. 
- Creating a Swift `Array` and an `NSArray` degrade at roughly the same rate between `O(log n)` and `O(n)`. `Swift` is faster than Foundation by roughly 30 times. This is a significant improvement over Swift 2.3, where it was roughly the same as Foundation!
- Adding items to the beginning of an array is considerably slower in a Swift `Array` than in an `NSArray`, which is around `O(1)`. Adding items to the middle of an array takes half the time in Swift `Array` than in an `NSArray`. Adding items to the end of a Swift `Array`, which is less than `O(1)`, is roughly 6 times faster than adding items to the end of an `NSArray`, which comes in just over `O(1)`.
- Removing objects is faster in a Swift `Array` than a `NSArray`. From the beginning, middle or end, removing an object degrades between `O(log n)` and `O(n)`. Raw time is better in `Swift` when you remove from the beginning of an Array, but the distinction is a matter of milliseconds.
- Looking up items in `Swift` is faster for the first time since its inception. Look ups by index grow at very close rates for both Swift `Arrays` and `NSArray` while lookup by object is roughly 80 times faster in Swift.

상기 글은 스위프트가 얼마나 최적화가 잘 되었냐는 글입니다. `Swift`쓰세요. 두번 쓰세요.

### Dictionaries
- 딕셔너리는 특정 순서와 상관없이 `key`값을 통해서 값을 찾을 수 있는 데이터 구조입니다.
- 딕셔너리는 `subscripting`을 지원하기 떄문에, `dictionary["hello"]` 와 같은 방법으로 `hello`라는 `key`값을 통해서 값에 접근 할 수 있습니다.
- `NSDictionary`와 `NSMutableDictionary`의 차이는 `Array`일 경우와 같습니다.
- `Swift`의 딕셔너리는 타입을 지정해줘야 합니다. (`NSDictionary`가 `NSObject`를 갖는 것 처럼)

##### 이럴때 쓰면 좋습니다.
- 순서가 없을 경우 딕셔너리를 사용하시면 됩니다. 
- 순서가 아닌 `key`값으로 접근 할 경우 좋습니다. `cats["Ellen"]`이렇게 사용할 경우 Optional이 return되기 때문에 `if let` 이나 `guard`로 풀어주는 것을 권장합니다.
```Swift
if let ellensCat = cats["Ellen"] {
    print("Ellen's cat is named \(ellensCat).")
} else {
    print("Ellen's cat's name not found!")
}
```

아래는 `Objective-C`와 `Swift`의 비교 결과입니다. 
- In raw time, creating Swift `Dictionaries` is roughly 6 times faster than creating `NSMutableDictionaries` but both degrade at roughly the same `O(n`) rate.
- Adding items to Swift `Dictionaries` is roughly 100 times faster than adding them to `NSMutableDictionaries` in raw time, and both degrade close to the best-case-scenario `O(1)` rate promised by Apple’s documentation.
- Removing items from Swift `Dictionaries` is roughly 8 times faster than removing items from `NSMutableDictionaries`, but the degradation of performance is again close to `O(1)` for both types.
- Swift is also faster at lookup, with both performing roughly at an `O(1)` rate. This version of Swift is the first where it beats Foundation by a significant amount.

퍼포먼스가 크게 차이가 나지 않는다고 합니다.

### Sets
- 셋은 순서가 없고 유일한 값을 저장하는 데이터 구조입니다. 중복으로 데이터를 등록 할 수 없습니다.
- `Set`에 선언되어있는 값들은 모두 같은 `type` 이어야 합니다.
- `Objective-c` 에서는 각각 `NSSet`과 `NSMutableSet`이 있습니다.

##### 이럴때 쓰면 좋습니다.
- 셋은 중복없이 유니크한 값을 중복없이 뽑을 경우 좋습니다. 
- `Snippet`을 하나 보겠습니다. 
```Swift
let names = ["John", "Paul", "George", "Ringo", "Mick", "Keith", "Charlie", "Ronnie"]
var stringSet = Set<String>() // 1
var loopsCount = 0
while stringSet.count < 4 {
    let randomNumber = arc4random_uniform(UInt32(names.count)) // 2
    let randomName = names[Int(randomNumber)] // 3
    print(randomName) // 4
    stringSet.insert(randomName) // 5
    loopsCount += 1 // 6
}
// 7
print("Loops: " + loopsCount.description + ", Set contents: " + stringSet.description)
```

1. 셋을 초기화 합니다.
2. 아이템 갯수중에 랜덤의 아이를 뽑습니다
3. 인덱스에 있는 아이를 가져옵니다
4. 로그를 남깁니다
5. `mutable set`에 값을 저장힙낟. 만약 값이 저장되어 있는 경우에는 값을 저장하지 않습니다.
6. 루프 카우트를 늘려서 루프를 다 돌게 합니다
7. 루프가 끝나면 값을 로그에 남깁니다

```Swift
John
Ringo
John
Ronnie
Ronnie
George
Loops: 6, Set contents: ["Ronnie", "John", "Ringo", "George"]
```
로그에는 6개의 값이 찍혔지만 결론적으로 `Set`에는 4개의 값이 들어가 있습니다.


## 덜 알려졌지만 있는 데이터 구조들
### NSCache
- `NSCache`를 사용하는 것은 `NSMutableDictionary`를 사용하는것돠 매우 유사합니다. 
- 다른 점은 메모리가 낮아질 경우 `NSCache`데이터는 지워 질 수 있습니다. 그리고 항상 재생성됩니다.
>캐시는 화면뒤에서 비동기적으로 자동으로 메모리에 상주 할 지, 남을지 결정합니다.

### NSCountedSet
- `NSMutableSet`를 상속받아서 만들어진 데이터 구조로, 얼마나 많은 오브젝트가 `mutable set`에 더해해졌는지 알 수 있게 해줍니다.
- 하단 코드를 참고해주세요

```Swift
let countedMutable = NSCountedSet()
for name in names {
    countedMutable.add(name)
    countedMutable.add(name)
}

let ringos = countedMutable.count(for: "Ringo")
print("Counted Mutable set: \(countedMutable)) with count for Ringo: \(ringos)")

Counted Mutable set: {(
    George,
    John,
    Ronnie,
    Mick,
    Keith,
    Charlie,
    Paul,
    Ringo
)}) with count for Ringo: 2
```
### NSOrderedSet
- `NSOrderedSet`은 개별적인 오브젝트들의 그룹을 순서대로 저장 할 수있게 해줍니다.
>`ordered set`을 `Array`의 대안으로 테스팅할 때 사용해보시면 배열보다 더 빠른것을 느낄 수 있습니다. 

### NSHashTable and NSMapTable
- `NSHashTable`은 `NSMutableSet`과 비슷하지만 몇가지 다른점이 있습니다
- `NSHashTable`은 오브젝트 뿐만 아니라 `non-object`또한 더할 수 있습니다. `NSHashTableOptions`로 메모리 또한 관리 할 수 있습니다.
- `NSMapTable`은 딕셔너리 같은 자료구조이지만, `NSHashTable`과 비슷하게 메모리를 관리한다는 측면이 있습니다.
