## Description
 
- 내부 객체에 대한 반복적인 비동기 참조 방법을 제공하는 프로토콜

## 주요 기능

+ 이 프로토콜을 따르면 for await in 구문을 사용할 수 있다.
+ 이 프로토콜을 따르려면 다음을 준수해야한다.
	+ AsyncIteratorProtocol을 따르는 비동기 이터레이터를 반환하는 makeAsnycIterator() 메서드를 구현한다.
		+ AsyncIteratorProtocol은 다음 요소를 반환하는 next() async -> Element? 메서드를 구현한다.
+ 그냥 여러 개의 태스크를 쓰면 되는거 아닌가?
	+ 태스크는 디스패치 순서대로 실행될 것이라고 보장할 수 없다. 순서를 보장해야 하는 비동기 작업이라면 AsyncSequece를 고민해볼만 하다.

## 코드 예시

```swift
struct Counter: AsyncSequence {
    typealias Element = Int
    let howHigh: Int


    struct AsyncIterator: AsyncIteratorProtocol {
        let howHigh: Int
        var current = 1


        mutating func next() async -> Int? {
            // A genuinely asynchronous implementation uses the `Task`
            // API to check for cancellation here and return early.
            guard current <= howHigh else {
                return nil
            }


            let result = current
            current += 1
            return result
        }
    }


    func makeAsyncIterator() -> AsyncIterator {
        return AsyncIterator(howHigh: howHigh)
    }
}

for await number in Counter(howHigh: 10) {
  print(number, terminator: " ")
}
// Prints "1 2 3 4 5 6 7 8 9 10 "
```


## References
- [`AsyncSequence`](https://developer.apple.com/documentation/swift/asyncsequence)