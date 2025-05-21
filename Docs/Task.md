>[!question]
>GQ1. Task는 무엇인가?

## 요약
- Task는 비동기로 실행될 수 있는 작업의 단위이다.
- 하나의 Task는 동시에 그 자체 하나만 실행될 수 있지만, 여러 개의 Task를 만들어 실행할 수 있고, 스위프트가 이를 스케쥴링한다.

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


## Task 취소

- Task는 협력적 취소 모델을 채택한다.
- 각 태스크는 적절한 지점에서 자신이 취소되었는지를 확인하고, 거기에 호응해, 어떤 태스크인지에 따라 적당한 취소 작업을 수행한다. 다운로드 작업, 큰 연산 작업 등 작업 유형에 따라 취소 방법이 달라질 것이다.
- 왜 협력적이라고 할까?
	- 시스템이 일방적으로 강제 종료하지 않는다. Task 내에서 최소되었음을 확인하고 알아서 대응해야 한다.
	- 즉, 태스크가 협력하지 않으면 취소되지 않는다.
- 취소 여부 확인 방법
	- 취소된 상태면 예외를 던지는 Task.checkCancellation()를 호출
	- Task.isCancelled (Bool) 값을 체크하고 clean-up 진행
- 취소된 것을 확인한 후 적당한 종료 작업을 한 뒤 제어권을 내려놓는 방법
	1. CancellationError를 태스크 내에서 던진다
	2. nil이나 빈 컬렉션을 반환한다
	3. 일부 완료된 작업을 반환한다

```swift
let photos = await withTaskGroup(of: Optional<Data>.self) { group in
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        let added = group.addTaskUnlessCancelled {
            guard !Task.isCancelled else { return nil }
            return await downloadPhoto(named: name)
        }
        guard added else { break }
    }


    var results: [Data] = []
    for await photo in group {
        if let photo { results.append(photo) }
    }
    return results
}
```
-  [`TaskGroup.addTaskUnlessCancelled(priority:operation:)`](https://developer.apple.com/documentation/swift/taskgroup/addtaskunlesscancelled\(priority:operation:\))를 사용하면 취소된 태스크가 그룹에 추가되는 것을 막을 수 있다. 추가 성공 여부를 Bool로 반환한다.
- 추가로 내부에서 태스크의 취소 여부를 확인하고 있다. 취소되었다면nil을 반환한다.
- 나중에 태스크 그룹에서는 nil을 반환하는 태스크를 무시한다 => 일부 ㄷ
## 구조화되지 않은 동시성 프로그래밍 Unstructured Concurrency

- 위와 같이 태스크 간의 위계를 따지지 않고 동시성 프로그래밍을 할 수도 있다. 이 경우 태스크가 어떤 태스크 그룹이나 부모 태스크에 속하지 않는다.
- 현재 액터에서 태스크를 만들려면 [`Task.init(priority:operation:)`](https://developer.apple.com/documentation/swift/task/init\(priority:operation:\)-7f0zv)을, 현재 액터와 분리되어 실행되는 태스크를 만들려면 [`Task.detached(priority:operation:)`](https://developer.apple.com/documentation/swift/task/detached\(priority:operation:\)-d24l)를 이용한다.

```swift
let newPhoto = // ... some photo data ...
let handle = Task {
    return await add(newPhoto, toGalleryNamed: "Spring Adventures")
}
let result = await handle.value
```

## Keywords
+ 파생된 키워드들을 작성

## References
- [The Swift Programming Language - Concurrency - Tasks-and-Task-Groups](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency#Tasks-and-Task-Groups)
- [Apple Developer Documentation - Task](https://developer.apple.com/documentation/swift/task/)
