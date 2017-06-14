# Collection Data Structures in Swift - KR
Swift의 Collection Data Structures를 소개합니다. 좋은 글이라 생각되어 번역합니다. 핵심적인 부분을 제외하고는 번역을 제외하고 의역했습니다. 본문은 [여기](https://www.raywenderlich.com/123100/collection-data-structures-swift-2)를 참고해주세요.

## Introduction
많은 데이터를 처리하는 어플리케이션을 만들고 있을 때를 생각해보세요. 그 데이터를 어디에 넣을 건가요? 어떻게 그 많은 데이터를 효율적으로 다룰 건가요?
이럴 때는 `Array`, `Dictionary`, `Set`과 같은 기본적인 데이터 구조들이 필요합니다. 이 `collection data structures`에 명확한 개념을 가질 경우 처리 속도는 어마어마하게 빨라 질 것입니다.
##### 튜토리얼의 순서는 다음과 같습니다.
1. 데이터 구조가 무엇인지 배우고, `Big-O` 표기법에 대해 배워봅니다.
2. 각 데이터 구조의 퍼포먼스를 확인해봅니다.
3. 기존의 Cocoa 데이터 구조와 Swift의 새로운 데이터 구조의 퍼포먼스를 비교해봅니다.
4. 추가의 팁으로 다른 구조들을 조금 더 알아봅니다.

## What is `Big-O` Notation?

컴퓨터에서 데이터를 처리할 때 걸리는 * 시간 * 또는 차지하는 * 메모리 공간 * 은 일반적으로 * 데이터의 양 * 에 비례하게 됩니다.
Big-O 표기법은 이때 필요한 시간, 공간을 * 점근적으로(asymptotically) * 표현하는 방법이며, 이렇게 표현한 것을 시간복잡도(time complexity), 공간복잡도(space complexity)라고 부릅니다.

아래는 자주 만날 수 있는 시간복잡도의 Big-O 표기법과 설명을 빠른 것부터 나열한 것입니다.
- `O(1)`: 제일 이상적입니다. 처리하는 데이터의 양(n)에 관계없이 상수 시간만큼 소요되는 경우입니다.
- `O(log n)`: 좋은 퍼포먼스를 보여주는 그래프입니다. 데이터 구조에 있는 아이템이 늘어나도 매우 천천히 연산의 횟수가 늘어나게 됩니다. (걸리는 시간도 마찬가지)
- `O(n)`: 중간 단계의 퍼포먼스를 보여주는 그래프입니다. 데이터 구조랑 연산횟수가 일차함수(linear)를 그리며 증가합니다.
- `O(n (log n))`: (데이터가 10M~100M 넘어갈 경우) 낮은 단계의 퍼포먼스를 보여주는 그래프입니다. 비교 기반 정렬(comparison based sort)의 시간복잡도 하한(lower bound)이기도 합니다.
- `O(n²) `: 데이터 구조에 있는 아이템 수의 제곱에 비례하여 늘어납니다. 여기서부턴 성능이 매우 떨어지고, 추천하지 않습니다.
- `O(2^n)`: 2의 데이터 구조의 개수 제곱만큼 증가합니다. 매우 나쁜 퍼포먼스를 보여줍니다. 원소 n개 집합의 모든 부분집합을 출력하는 경우의 시간복잡도와 같습니다.
- `O(n!)`: 최악의 케이스입니다. 이렇게 코딩이 될 경우에는 답이 없습니다.

![Big-o](Images/big-o.png)

## Common iOS Data Structures
`Array`, `Dictionary`, `Set`이 가장 흔한 데이터 구조입니다. 각각의 구조와 성능에 대해 알아봅니다. 또한 `Objective-C`와 `Swift`에서 각각의 데이터 구조를 비교해봅니다.

### Array
- `Array`는 순서를 가지고 있는 배열입니다. `index`를 통해서 각각의 아이템에 접근할 수 있습니다. `index`를 가지고 아이템을 가져오는 것을 `subscripting`이라고 합니다.
- `Objective-C`의 `NSArray`는 `Swift`에서 `let`으로 선언된 `Array`와 같습니다. `NSMutableArray`는 `var`로 선언된 `Array`와 같습니다.
- 특정한 `index`를 가지는 값에 접근할 때의 시간복잡도는, 최악의 케이스는 `O(log n)`이지만 보통 `O(1)`입니다.
- 위치를 모르는 아이템을 찾을 경우의 시간복잡도는, 최악은 `O(n (log n))`이지만 보통 `O(n)`입니다.
- 데이터를 삭제하거나 넣는 경우의 시간복잡도는, 최악은 `O(n (log n))`이지만 보통 `O(1)`입니다.

##### 이럴 때 쓰면 좋습니다.
- `item`이 어디 있는지 아는 경우, 검색은 빠를 것입니다.
- `item`이 어디 있는지 모를 경우, 처음부터 끝까지 찾아야 하므로 검색이 느립니다.
- `item`을 어디에 넣고 삭제해야 하는지 알고 있다면 어렵지 않겠지만, 배열의 다른 `item`에도 계속해서 적용해야 하는 경우는 더 긴 시간이 필요합니다.

>`Swift`는 2015년 12월부터 오픈소스입니다. [Swift Code](https://swift.org/source-code/)로 들어가셔서 구조가 어떻게 되어있는지 확인해보시는 것도 좋은 방법입니다.

아래는 `Objective-C`와 `Swift`의 비교 결과입니다.
- Swift의 `Array`와 Foundation의 `NSArray`를 생성하는 시간은 둘 다 `O(log n)`과 `O(n)` 사이의 시간복잡도를 보여줬습니다. 다만 `Array`가 `NSArray`보다 대략 30배나 빨랐습니다. 이것은 Swift 2.3 버전에서는 거의 비슷했던 것에서 상당히 발전한 것입니다!
- 배열의 시작 부분에 아이템을 추가하는 것은 `Array`가 `NSArray`보다 꽤 느렸습니다. (시간복잡도는 `O(1)`) 배열의 중간에 아이템을 추가하는 것은 `Array`가 `NSArray`의 절반밖에 걸리지 않았습니다. 배열의 마지막에 아이템을 추가하는 것은 `O(1)`보다 빠른 결과를 보여준 `Array`가 그렇지 않았던 `NSArray`보다 대략 6배 빠른 속도를 보여줬습니다.  
- 아이템을 삭제하는 것은 Swift의 `Array`가 `NSArray`보다 빨랐습니다. 배열의 위치가 처음이건, 중간이건, 마지막이건 `O(log n)`과 `O(n)` 사이의 시간복잡도를 가졌습니다. `Swift`에서는 배열의 위치가 처음일 때 조금 더 빨랐지만, 그 차이는 milliseconds 수준이었습니다.
- 아이템을 참조하는 것은 배열의 앞부분에서는 `Swift`가 빠릅니다. index가 증가할수록 `Array`와 `NSArray` 모두 비슷한 속도로 아주 천천히 증가합니다. 다만 index가 아니라 object를 통한 참조일 때는 `Swift`에서 80배 정도 빠른 속도를 보여줍니다.

상기 글은 스위프트가 얼마나 잘 최적화되었는지 보여줍니다. `Swift` 쓰세요. 두 번 쓰세요.

### Dictionary
- Dictionary(사전)는 특정 순서와 상관없이 `key`를 통해서 값을 찾을 수 있는 데이터 구조입니다.
- Dictionary는 `subscripting`을 지원하기 때문에, `dictionary["hello"]`와 같은 방법으로 `hello`라는 `key`를 통해서 값에 접근할 수 있습니다.
- `NSDictionary`와 `NSMutableDictionary`의 차이는 `Array`의 경우와 같습니다.
- `Swift`의 Dictionary는 타입을 지정해줘야 합니다. (`NSDictionary`가 `NSObject`를 갖는 것처럼)

##### 이럴 때 쓰면 좋습니다.
- 데이터에 순서가 없으면 Dictionary를 사용하시면 됩니다.
- 순서가 아닌 `key`로 접근할 경우 좋습니다. `cats["Ellen"]`과 같이 사용할 경우 Optional이 return 되기 때문에 `if let` 이나 `guard`로 풀어주는 것을 권장합니다.
```Swift
if let ellensCat = cats["Ellen"] {
    print("Ellen's cat is named \(ellensCat).")
} else {
    print("Ellen's cat's name not found!")
}
```

아래는 `Objective-C`와 `Swift`의 비교 결과입니다.
- Swift의 `Dictionary`를 생성하는 것은 `NSMutableDictionary`를 생성하는 것보다 대략 6배 정도 빨랐습니다. 그러나 두 경우 모두 시간복잡도는 `O(n)`으로 차이가 없었습니다.
- Swift의 `Dictionary`에 아이템을 추가하는 것은 `NSMutableDictionary`에 추가할 때보다 100배 정도 빨랐습니다. 시간복잡도는 `O(1)`에 가까운 이상적인 결과를 보여줬습니다.
- Swift의 `Dictionary`에서 아이템을 제거하는 것은 `NSMutableDictionary`에서 제거할 때보다 8배 정도 빨랐습니다. 시간복잡도는 `O(1)`에 가까웠습니다.
- Swift의 `Dictionary`는 참조 역시 빨랐지만, 역시 시간복잡도는 둘 다 `O(1)`였습니다. Swift 3.0은 Foundation보다 상당한 퍼포먼스 향상을 보여준 첫 버전입니다.

시간복잡도에서는 차이가 없지만, Swift 3.0에서 상당한 속도 향상이 있었습니다.


### Set
- `Set`(집합)은 순서가 없고 유일한 값을 저장하는 데이터 구조입니다. 중복으로 데이터를 등록할 수 없습니다.
- `Set`에 선언되어있는 값들은 모두 같은 `type` 이어야 합니다.
- `Objective-c` 에서는 각각 `NSSet`과 `NSMutableSet`이 있습니다.

##### 이럴 때 쓰면 좋습니다.
- `Set`은 중복이 없는 유일한 값들을 골라낼 때 유용합니다.
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

1. `Set`을 초기화합니다.
2. 아이템을 랜덤으로 뽑습니다.
3. 해당 index에 있는 값을 가져옵니다.
4. 로그를 남깁니다.
5. `mutable set`에 값을 저장합니다. 만약 값이 이미 저장되어 있는 경우에는 다시 저장하지 않습니다.
6. 루프 카운트를 늘려서 루프를 다 돌게 합니다.
7. 루프가 끝나면 값을 로그에 남깁니다.

```Swift
John
Ringo
John
Ronnie
Ronnie
George
Loops: 6, Set contents: ["Ronnie", "John", "Ringo", "George"]
```
로그에는 6개의 값이 찍혔지만, 결론적으로 `Set`에는 4개의 값이 들어가 있습니다.


## 덜 알려진 데이터 구조들
### NSCache
- `NSCache`를 사용하는 것은 `NSMutableDictionary`를 사용하는 것과 매우 유사합니다.
- 다른 점은 메모리가 낮아질 경우 `NSCache` 데이터는 지워질 수 있습니다. 그리고 항상 재생성됩니다.
>캐시는 화면 뒤에서 비동기적으로 자동으로 메모리에 상주 할지, 남을지 결정합니다.

### NSCountedSet
- `NSMutableSet`을 상속받아서 만들어진 데이터 구조로, 얼마나 많은 오브젝트가 `mutable set`에 더해졌는지 알 수 있게 해줍니다.
- 하단 코드를 참고해주세요.

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
- `NSOrderedSet`은 개별적인 오브젝트들의 그룹을 순서대로 저장할 수 있게 해줍니다.
>`ordered set`을 `Array`의 대안으로 테스팅할 때 사용해보시면 배열보다 더 빠른 것을 알 수 있습니다.

### NSHashTable and NSMapTable
- `NSHashTable`은 `NSMutableSet`과 비슷하지만 몇 가지 다른 점이 있습니다
- `NSHashTable`은 오브젝트 뿐만 아니라 `non-object` 또한 더할 수 있습니다. `NSHashTableOptions`로 메모리 또한 관리할 수 있습니다.
- `NSMapTable`은 dictionary 같은 자료구조이지만, `NSHashTable`과 비슷하게 메모리를 관리한다는 측면이 있습니다.

---

### Author

**[younatics 🇰🇷](https://younatics.github.io)**
