>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- SwiftUI 뷰를 UIKit 하이어아키에 통합하여 사용하고 싶을 때 활용

## 주요 기능
+ 실제 활용을 작성

## 코드 예시
+ 실제 코드 예시를 작성


## 알려진 제약
- Binding 등을 넘길 때 복잡해짐. Binding 객체에 getter, setter를 넣어서는 가능
    ```swift
	let swiftUITextField = CustomSwiftUITextView(text: Binding(get: { self.text }, set: { newValue in self.text = newValue })
    ```
- 다른 방법으로 클로져를 넘겨서 UIKit VC와 통신하게 할 수 있음
	- 참고: https://stackoverflow.com/a/64730736
- 가능한 Presentation 역할을 하는 뷰만 호스팅하는게 좋아보임

## ​Keywords
+ 파생된 키워드들을 작성

## References
- https://developer.apple.com/documentation/swiftui/uihostingcontroller
- https://www.hohyeonmoon.com/blog/swiftui-tutorial-uikit
- 