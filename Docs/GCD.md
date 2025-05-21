>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- libdispatch라고도 불린다.
- 여러 작업을 태스크 단위로 큐(FIFO)에서 처리한다.
- C로 작성되었고 objc 뿐 아니라 swift에서도 브릿징하여 사용 가능하다. 대부분의 POSIX 운영체제와 리눅스로 포팅되었다.
- 네 가지 경우의 수가 가능하다.
    - SerialQueue.async
    - SerialQueue.sync
    - ConcurrentQueue.asnyc
    - ConcurrentQueue.sync

## SerialQueue와 ConcurrentQueue
- SerialQueue
	- 단일 쓰레드를 사용한다.
	- 각 태스크가 순차적으로 수행된다.
- ConcurrentQueue
    - 멀티 쓰레드를 사용한다.
    - 각 태스크가 여러 쓰레드에 분배되어 동시에 수행될 수 있다.

## Sync와 Async
- Sync
    - sync는 호출한 쪽의 쓰레드를 블로킹한다.
- Async
    - async는 호출한 쪽의 쓰레드를 블로킹하지 않는다.
- Kodeco 책의 정리
    - “In other words, a task being synchronous or not speaks to the source of the task. Being serial or concurrent speaks to the destination of the task.”
    - 동기/비동기는 작업을 만든 쪽에 대한 이야기이고, 순차/동시성은 작업이 (디스패치되어) 도달한 곳에 대한 이야기이다.

## 코드 예시
+ 실제 코드 예시를 작성

## Keywords
+ 파생된 키워드들을 작성

## References
- https://swiftlang.github.io/swift-corelibs-libdispatch/
- https://developer.apple.com/documentation/DISPATCH