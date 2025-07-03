>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- 개요와 설명을 작성

## 주요 기능
+ 실제 활용을 작성

## 코드 예시
```swift
import Foundation
import SwiftUI
import UIKit


struct CounterViewControllerRepresentable: UIViewControllerRepresentable {
    @Binding var count: Int
    
    func makeUIViewController(context: Context) -> CounterViewController {
        let vc = CounterViewController()
        vc.buttonDidTapClosure = context.coordinator.updateCount
        return vc
    }
    
    func updateUIViewController(_ uiViewController: CounterViewController, context: Context) {
        uiViewController.numberLabel.text = "\(count)"
    }
    
    func makeCoordinator() -> Coordnator {
        Coordnator(count: $count)
    }
    
    class Coordnator {
        @Binding var count: Int
        
        init(count: Binding<Int>) {
            self._count = count
        }
        
        func updateCount() {
            count += 1
        }
    }
}


```

## Keywords
+ 파생된 키워드들을 작성

## References
- https://developer.apple.com/documentation/swiftui/uiviewcontrollerrepresentable
- https://www.hohyeonmoon.com/blog/swiftui-tutorial-uikit
