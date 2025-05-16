## Description
 
- 내부 객체에 대한 반복적인 참조 방법을 제공하는 프로토콜

## 주요 기능

+ 이 프로토콜을 따르면 for-in 구문이나 contains 메서드를 사용할 수 있다.
+ 이 프로토콜을 따르려면 다음을 준수해야한다.
	+ 이터레이터를 반환하는 makeIterator() 메서드를 구현하거나,
	+ 자기 자신이 IteratorProtocol을 준수
		+ IteratorProtocol을 따르려면 다음 요소를 반환하는 next() -> Element? 메서드를 구현한다.

## 코드 예시

```swift
struct Countdown: Sequence, IteratorProtocol {
    var count: Int


    mutating func next() -> Int? {
        if count == 0 {
            return nil
        } else {
            defer { count -= 1 }
            return count
        }
    }
}


let threeToGo = Countdown(count: 3)
for i in threeToGo {
    print(i)
}
// Prints "3"
// Prints "2"
// Prints "1"
```

## References
- [Sequence](https://developer.apple.com/documentation/swift/sequence)