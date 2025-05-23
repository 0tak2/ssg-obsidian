>[!question]
>GQ1. Sendable

## Description
- 여러 쓰레드 간에 안전하게 전달할 수 있는 타입임을 나타내는 프로토콜
- Sendable을 따르려면 다음의 조건을 따라야 한다.
	- Value types
	- Reference types with no mutable storage
	- Reference types that internally manage access to their state
	- Functions and closures (by marking them with `@Sendable`)
- 컴파일러가 해당 타입이 Sendable인지 검사하지 않도록 우회하려면 `@unchecked Sendable`를 
## 주요 기능
+ 실제 활용을 작성

## 코드 예시
+ 실제 코드 예시를 작성

## Keywords
+ 파생된 키워드들을 작성

## References
- 참고한 레퍼런스를 작성 (예 : Apple의 공식 문서)