>[!question]
>GQ1. UIKit에서 SwiftUI 뷰를 쓰려면 어떻게 해야하지?

## Description
- SwiftUI 뷰를 UIKit 하이어아키에 통합하여 사용하고 싶을 때 활용

## 코드 예시

```swift
import UIKit
import SwiftUI

class MainViewController: UIViewController {
    var messageView: MessageView!
    var message: String = "Hello, World" {
        didSet {
            messageLabel.text = message
        }
    }
    let messageLabel = UILabel()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.backgroundColor = .systemBackground
        
        messageView = MessageView(message: Binding(get: {
            self.message
        }, set: { newValue in
            self.message = newValue
        }))
        
        let messageViewHostingContoller = UIHostingController(rootView: messageView)
        
        view.addSubview(messageViewHostingContoller.view)
        view.addSubview(messageLabel)
        
        messageViewHostingContoller.view.translatesAutoresizingMaskIntoConstraints = false
        messageLabel.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            messageViewHostingContoller.view.topAnchor.constraint(equalTo: view.topAnchor),
            messageViewHostingContoller.view.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            messageViewHostingContoller.view.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            messageViewHostingContoller.view.heightAnchor.constraint(equalToConstant: 150),
        ])
        
        NSLayoutConstraint.activate([
            messageLabel.topAnchor.constraint(equalTo: messageViewHostingContoller.view.bottomAnchor, constant: 16),
            messageLabel.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            messageLabel.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            messageLabel.bottomAnchor.constraint(equalTo: view.bottomAnchor),
        ])
    }
}
```

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
