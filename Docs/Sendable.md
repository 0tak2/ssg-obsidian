>[!question]
>GQ1. Sendable

## Description
- 여러 쓰레드 간에 안전하게 전달할 수 있는 타입임을 나타내는 프로토콜
- Sendable을 따르려면 다음의 조건을 따라야 한다.
	- Value types
	- Reference types with no mutable storage
	- Reference types that internally manage access to their state
	- Functions and closures (by marking them with `@Sendable`)
- 컴파일러가 해당 타입이 Sendable인지 검사하지 않도록 우회하려면 `@unchecked Sendable`를 쓴다.

## 코드 예시

```swift
struct Pineapple: Sendable { … } // conforms to Sendable because its a value type
class Chicken: Sendable { } // ERROR. cannot conform to Sendable because its an unsynchronized reference type.

// contains an array of Sendable types, therefore is Sendable
struct Crate: Sendable {
  var pineapples: [Pineapple]
}

// stored property 'flock' of 'Sendable'-conforming struct 'Coop' has non-sendable type '[Chicken]'
struct Coop: Sendable {  // ERROR.
  var flock: [Chicken]
}

// Can be Sendable if a final class has immutable storage
final class Chicken: Sendable {
  let name: String
  var currentHunger: HungerLevel // 'currentHunger' is mutable, therefore Chicken cannot be Sendable
}
```

## References
- [Concurrency - Sendable Types](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Sendable-Types)
- [Sendable](https://developer.apple.com/documentation/swift/sendable)
- [WWDC21 - Eliminate data races using Swift Concurrency](https://developer.apple.com/videos/play/wwdc2022/110351/)