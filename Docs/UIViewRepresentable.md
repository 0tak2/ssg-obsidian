>[!question]
>GQ1. SwiftUI에서 UIKit View를 쓰려면 어떻게 해야하지?

## Description
- UIKit의 UIView 객체를 SwiftUI에서 관리하고 싶을 때 사용하는 프로토콜

## 주요 기능
+ [`func makeUIView(context: Self.Context) -> Self.UIViewType`](https://developer.apple.com/documentation/swiftui/uiviewrepresentable/makeuiview\(context:\)): Creates the view object and configures its initial state.

- [`func updateUIView(Self.UIViewType, context: Self.Context)`](https://developer.apple.com/documentation/swiftui/uiviewrepresentable/updateuiview\(_:context:\)): Updates the state of the specified view with new information from SwiftUI.

-  [`center`](https://developer.apple.com/documentation/UIKit/UIView/center), [`bounds`](https://developer.apple.com/documentation/UIKit/UIView/bounds), [`frame`](https://developer.apple.com/documentation/UIKit/UIView/frame), [`transform`](https://developer.apple.com/documentation/UIKit/UIView/transform) 등 속성은 SwiftUI에서 관리하므로 직접 지정하면 안됨

## 코드 예시
```swift
import Foundation
import SwiftUI
import UIKit

/// - MARK: UIViewRepresentable
struct UITextFieldRepresentable: UIViewRepresentable {
    @Binding var text: String
	
	/// 처음 렌더링될 때 호출됨
    func makeUIView(context: Context) -> UITextField {
        let textField = UITextField(frame: .zero)
        textField.placeholder = "Input some message here..."
        textField.delegate = context.coordinator
        return textField
    }

	/// 상태 변경 시 호출됨
    func updateUIView(_ uiTextField: UITextField, context: Context) {
        uiTextField.text = text
    }

	/// 가장 먼저 호출됨
    func makeCoordinator() -> Coordinator {
        return Coordinator(text: $text)
    }
    
    class Coordinator: NSObject, UITextFieldDelegate {
        @Binding var text: String
        
        init(text: Binding<String>) {
            self._text = text
        }
        
        func textFieldDidChangeSelection(_ textField: UITextField) {
            text = textField.text ?? ""
        }
    }
}


struct ContentView: View {}
    @State private var message = ""
    
    var body: some View {
        VStack {
            Text("Your Input: \(message)")
            UITextFieldRepresentable(text: $message)
        }
        .padding()
    }
}

```

## Feedbacks
- 제옹: 브릿지하는데 자원 비용이 들기 때문에, AR을 활용할 때 사용하려는 거라면 그냥 모두 UIKit으로 짜는게 나을 수 있다. 신중하게 고민.

## Keywords
+ 파생된 키워드들을 작성

## References
- https://developer.apple.com/documentation/swiftui/uiviewcontrollerrepresentable
- https://www.hohyeonmoon.com/blog/swiftui-tutorial-uikit
