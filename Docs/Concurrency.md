>[!question]
>- Swift Concurrency는 무엇인가?
>- 어떨 때 어떤 목적으로 사용해야 하는가?
>- 이전 방법과의 차이는 무엇인가?
>    - 비동기성: Completion Handler로 클로져를 넘기던 예전 방식
>    - 병렬성: GCD를 이용해 여러 작업을 동시에 처리하던 예전 방식

## 요약
Swift는 비동기 프로그래밍과 병렬 프로그래밍을 수행할 수 있는 구조화된 방법을 빌트인으로 제공하며, 이것이 Swift의 Concurrency이다.

## 동시성, 비동기성, 병렬성의 개념

- 물론 동시성, 비동기성, 병렬성은 엄밀히 따지면 모두 다른 개념이기는 하다.
- 동시성은 물리적으로 실제 그렇든 그렇지 않든, 동시에 여러 작업이 수행되거나 수행되는 것처럼 보이게 하는 기술이다. 비동기 프로그래밍과 병렬 프로그래밍은 이를 위해 필요하다.
- 비동기 프로그래밍은 작업을 시작한 후 종료되길 기다리지 않고 (블로킹), 다음 작업이 수행될 수 있도록 하는 방식이다.
- 병렬 프로그래밍은 실제 물리적으로 여러 자원을 이용해 동시에 여러 작업을 수행하는 방식이다.

## 스레드

- 스위프트의 동시성 모델은 스레드의 상위 레이어에 있는 추상화 계층이다. 따라서 유저는 스레드를 직접적으로 조작하지 않는다.
- 스위프트의 비동기함수는 동작하는 스레드를 포기하고 반환할 수 있고, 그럼 다른 비동기 함수가 앞선 비동기 함수가 블로킹된 시간 동안 해당 스레드에서 실행될 수 있다.
- 블로킹되었던 함수가 다시 실행될 때, 원래 실행되던 스레드가 아닐 수 있다. 동일 스레드에서 실행된다고 보장하지 않는다.

## 비동기 함수의 정의

```swift
func functionName(/* parameters */) async -> ReturnType {
	let result = /* some async jobs */
	return result
}

func functionNameThrowable(/* parameters */) async throws -> ReturnType {
	let result = try /* some async jobs including throwable method calls */
	return result
}
```

- 함수 시그니쳐에 async 키워드를 포함한다.
- 그럼 해당 함수가 특정 부분에서 기다려도 앱의 다른 코드는 실행된다. (논 블로킹)

## 비동기 함수의 실행

- 갤러리의 이미지 리스트를 가져오고, 그 중 첫번째 이미지를 다운로드하는 가상 예제를 생각해보자.
- listPhotos(inGallery:)와 downloadPhoto(named:)는 모두 네트워크 작업을 포함하여 비동기 함수로 정의했다고 가정한다.

```swift
let photoNames = await listPhotos(inGallery: "Summer Vacation")
let sortedNames = photoNames.sorted()
let name = sortedNames[0]
let photo = await downloadPhoto(named: name)
show(photo)
```

1. 코드는 await 키워드 전까지 수행되고 잠시 중단된다. 그 시점에 다른 동시성 작업은 여전히 실행되고 있다.
2. await 키워드가 붙은 표현식의 평가가 완료되면 (리턴되면) 다시 다음 라인이 실행된다.
3. 다른 동기 코드는 중단 없이 실행된다.

- 즉, async는 비동기 함수를 정의할 때 사용하는 키워드라면, await 키워드는 비동기 함수를 실행할 때 사용하는 키워드이다.
- await 키워드는 해당 지점에서 코드가 잠시 중단될 수 있음을 나타낸다. 중단이라고 간단하게 이야기했지만, 다른 말로 *스레드 양보(yielding a thread)*이다. 내부적으로는 이 지점에서 해당 스레드에서 다른 코드가 실행되게 된다.

### 실행 가능한 조건

- await은 해당 지점 코드를 중단하기 때문에 특정 컨텍스트에서만 사용 가능하다.
	- 비동기함수, 메서드, 속성 안에서
	- `@main`이 지정된 구조체, 클래스, 열거형의 static `main()` 메서드 안에서
	- 구조화되지 않은 자식 Task 안에서 (unstructured child task)

### 명시적으로 스레드 양보

[`Task.yield()`](https://developer.apple.com/documentation/swift/task/3814840-yield)를 실행해 명시적으로 중단점을 잡을 수 있다. 반복문과 같이 작업에 오랜 시간이 걸리는 경우 고려해볼 수 있다. 스위프트가 해당 태스크와 다른 태스크 사이 균형을 맞추는 것을 도와준다.

## 비동기 Sequence

for-await-in 구문을 이용해 컬렉션의 요소를 비동기적으로 순회할 수 있다.

```swift
import Foundation


let handle = FileHandle.standardInput
for try await line in handle.bytes.lines {
    print(line)
}
```

- for-in 구문을 이용해 순회하기 위해 [`Sequence`](Docs/Sequence)를 따라야하는 것처럼, for-await-in 구문을 이용해 순회하려면 [`AsyncSequence`](Docs/AsyncSequece)를 따라야한다.

## 비동기 함수의 병렬 호출

- 이렇게 한다면 일련의 비동기 함수를 순차호출하게 된다. 다만 순차 호출할 이유가 없어보인다.
	```swift
	let firstPhoto = await downloadPhoto(named: photoNames[0])
	let secondPhoto = await downloadPhoto(named: photoNames[1])
	let thirdPhoto = await downloadPhoto(named: photoNames[2])
	
	
	let photos = [firstPhoto, secondPhoto, thirdPhoto]
	show(photos)
	```
- 이렇게 하면 병렬적으로 동시에 호출할 수 있다. (이전 호출 결과를 기다리지 않는 것이고, 유휴 자원이 부족하면 모든 작업이 동시 호출되지는 않을 수 있다.)
	```swift
	async let firstPhoto = downloadPhoto(named: photoNames[0])
	async let secondPhoto = downloadPhoto(named: photoNames[1])
	async let thirdPhoto = downloadPhoto(named: photoNames[2])
	
	
	let photos = await [firstPhoto, secondPhoto, thirdPhoto]
	show(photos)
    ```
- async let 키워드는 암시적으로 자식 [Task](Docs/Task)를 만든다.

## 다른 비동기 작업 수행 방식
- Swift Concurrency 이전 등장해 지금도 사용되는 [GCD](Docs/GCD)
- 별개의 EventLoop 위에서 동작하는 [SwiftNIO](Docs/SwiftNIO)
- Apple의 다른 프레임워크는 별도의 쓰레드를 직접 사용할 수도 있음

## Keywords
+ [Task](Docs/Task)
+ [Actor](Docs/Actor)
+ [Sendable](Docs/Sendable)
+ [GCD](Docs/GCD)
+ [SwiftNIO](Docs/SwiftNIO)

## References
- [The Swift Programming Language - Concurrency](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency)
- 