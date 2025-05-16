>[!question]
>GQ1. Task는 무엇인가?

## 요약
- Task는 비동기로 실행될 수 있는 작업의 단위이다.
- 하나의 Task는 동시에 그 자체 하나만 실행될 수 있지만, 여러 개의 Task를 만들어 실행할 수 있고, 스위픝트가 이를 스케쥴링한다.

## 구조화된 동시성 프로그래밍 Structured Concurrency

- Task는 다른 Task와 함께 TaskGroup에 속할 수 있고, 자식 Task를 가질 수 있다.
- 이런 식으로 여러 비동기 작업 단위를 구조화할 수 있다는 점에서 이런 식으로 접근하는 것을 구조화된 동시성(structured concurrency)이라고 한다.

```swift
let photos = await withTaskGroup(of: Data.self) { group in // of는 TaskGroup의 제네릭. 속한 Task들의 리턴 타입
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        group.addTask {
            return await downloadPhoto(named: name)
        }
    }


    var results: [Data] = []
    for await photo in group {
        results.append(photo)
    }


    return results
}
```

- 이런 식으로 구조화하면 다음과 같은 장점이 있다.
	- 상위 작업에서 하위 작업이 완료될 때까지 기다리는게 명시된다.
	- 하위 작업에 더 높은 우선순위를 설정하면 상위 작업의 우선순위가 자동으로 올라간다.
	- 상위 작업이 취소되면 각 하위 작업도 자동으로 취소된다
- 
## Keywords
+ 파생된 키워드들을 작성

## References
- [The Swift Programming Language - Concurrency - Tasks-and-Task-Groups](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency#Tasks-and-Task-Groups)
- [Apple Developer Documentation - Task](https://developer.apple.com/documentation/swift/task/)
