>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- UIKit의 UIViewController 객체를 SwiftUI에서 관리하고 싶을 때 사용하는 프로토콜

## 주요 기능
- [`func makeUIViewController(context: Self.Context) -> Self.UIViewControllerType`](https://developer.apple.com/documentation/swiftui/uiviewcontrollerrepresentable/makeuiviewcontroller\(context:\)): Creates the view controller object and configures its initial state.
- [`func updateUIViewController(Self.UIViewControllerType, context: Self.Context)`](https://developer.apple.com/documentation/swiftui/uiviewcontrollerrepresentable/updateuiviewcontroller\(_:context:\)): Updates the state of the specified view controller with new information from SwiftUI.

**Required**

-  [`center`](https://developer.apple.com/documentation/UIKit/UIView/center), [`bounds`](https://developer.apple.com/documentation/UIKit/UIView/bounds), [`frame`](https://developer.apple.com/documentation/UIKit/UIView/frame), [`transform`](https://developer.apple.com/documentation/UIKit/UIView/transform) 등 속성은 SwiftUI에서 관리하므로 직접 지정하면 안됨

## 코드 예시
```swift
import Foundation
import SwiftUI
import UIKit

/// MARK: UIKit View Controller
class CounterViewController: UIViewController {
    let numberLabel = UILabel()
    let incrementButton = UIButton(type: .system)
    var buttonDidTapClosure: (() -> Void)?

    override func viewDidLoad() {
        super.viewDidLoad()
        
        numberLabel.text = String(0)
        numberLabel.textAlignment = .center
        incrementButton.setTitle("Increment", for: .normal)
        incrementButton.addTarget(self, action: #selector(buttonDidTap), for: .touchUpInside)
        
        view.addSubview(numberLabel)
        view.addSubview(incrementButton)
        
        numberLabel.translatesAutoresizingMaskIntoConstraints = false
        incrementButton.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            numberLabel.topAnchor.constraint(equalTo: view.topAnchor),
            numberLabel.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            numberLabel.trailingAnchor.constraint(equalTo: view.trailingAnchor)
        ])
        
        NSLayoutConstraint.activate([
            incrementButton.topAnchor.constraint(equalTo: numberLabel.bottomAnchor, constant: 15),
            incrementButton.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            incrementButton.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            incrementButton.bottomAnchor.constraint(lessThanOrEqualTo: view.bottomAnchor),
        ])
    }
    
    @objc private func buttonDidTap() {
        buttonDidTapClosure?()
    }
}

/// MARK: UIKit View Controller Representable
struct CounterViewControllerRepresentable: UIViewControllerRepresentable {
    @Binding var count: Int
    
    func makeUIViewController(context: Context) -> CounterViewController {
        let vc = CounterViewController()
        vc.buttonDidTapClosure = context.coordinator.updateCount
        return vc
    }
    
    func updateUIViewController(_ uiViewController: CounterViewController, context: Context) { // MARK: Updates the state of the specified view with new information from SwiftUI
        uiViewController.numberLabel.text = "\(count)"
    }
    
    func makeCoordinator() -> Coordnator {
        Coordnator(count: $count)
    }
    
    class Coordnator { // MARK: UIKit -> SwiftUI 데이터 전달시 makeCoordinator 사용. Coordnator는 각종 UIKit 객체의 Delegate가 될 수도 있음
        @Binding var count: Int
        
        init(count: Binding<Int>) {
            self._count = count
        }
        
        func updateCount() {
            count += 1
        }
    }
}

struct ContentView: View {
    @State private var count = 0
    
    var body: some View {
        VStack {
            HStack {
                ForEach(0..<count, id: \.self) { count in
                    Image(systemName: "globe")
                        .imageScale(.large)
                        .foregroundStyle(.tint)
                }
            }
            
            CounterViewControllerRepresentable(count: $count)
        }
        .padding()
    }
}
```

## Keywords
+ 파생된 키워드들을 작성

## References
- https://developer.apple.com/documentation/swiftui/uiviewcontrollerrepresentable
- https://www.hohyeonmoon.com/blog/swiftui-tutorial-uikit
