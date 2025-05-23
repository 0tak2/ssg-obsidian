>[!question]
>GQ1. Actor가 무엇인가?

## Description

- Class와 같이 참조 타입을 정의한다.
- 다만 Concurrency 환경에서도 데이터 레이스 없이 안전하게 다룰 수 있는 공유 가능한 가변 상태를 포함할 수 있다.

## 주요 기능

+ Actor는 Class와 거의 동일하지만, 내부의 가변 상태를 변경하는 작업은 따로 명시하지 않더라도 한 시점에 한 번만 할 수 있다.
+ 즉 동시에 상태 변경이 이뤄지는 경우 암묵적으로 시리얼하게 수행된다.
+ 가변 상태나, 상태를 변경하는 메서드를 외부에서 호출할 떄에는 await 키워드를 이용해야 한다. 메서드 시그니쳐에 async 키워드가 없더라도 그렇다.

## [코드 예시](https://github.com/0tak2/ios-study/tree/main/self-study/ConcurrentPlayground.playground)

#### 클래스로 카운터를 구현한 뒤 동시 접근

```swift
// MARK: - Without Actor
class ConterClass {
    var count: Int = 0
    
    func incremnet() {
        count += 1
    }
    
    func longRunningTask() {
        for _ in (0..<10000) {
            count += 1
        }
    }
}

let counter = ConterClass()

for _ in (0..<10) {
    // Main Thread
    counter.longRunningTask()

    Task.detached {
        counter.longRunningTask()
    }

    Task.detached {
        counter.longRunningTask()
    }
}

sleep(3)

print(counter.count) // 300000? => 매번 다름. 252990, 240934, 252995, ...
```

- 결과로 300000이 출력되길 기대했지만 300000보다 작은 값이 출력된다.
- 데이터레이스가 발생한 것이다. 특정 쓰레드가 연산 결과를 메모리에 커밋했더라도, 이전 연산 결과를 가지고 연산을 시작한 다른 쓰레드로의 연산 결과가 그 뒤에 덮어 씌워진 것이다.

#### 액터로 상태 변경을 격리시키기

```swift
// MARK: - Actor

actor CounterActor {
    var count: Int = 0
    
    func incremnet() {
        count += 1
    }
    
    func longRunningTask() {
        for _ in (0..<10000) {
            count += 1
        }
    }
    
    func getCount() -> Int {
        return count
    }
}

let counter2 = CounterActor()

for _ in (0..<10) {
    // Main Thread
    await counter2.longRunningTask()

    Task.detached {
        await counter2.longRunningTask()
    }

    Task.detached {
        await counter2.longRunningTask()
    }
}

sleep(3)

print(await counter2.getCount()) // 300000
```

- 예상한 결과가 출력된다.

## Keywords
+ 파생된 키워드들을 작성

## References
- [Concurrency - Actors](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Actors)
- [WWDC21 - Protect mutable state with Swift actors](https://developer.apple.com/videos/play/wwdc2021/10133/)
