>[!question]
>GQ1. UIGraphicsImageRenderer가 무엇일까?
>GQ2. UIGraphicsImageRenderer로는 무엇을 그릴 수 있을까?

## Description
- Core Graphic 지원 이미지를 그릴 수 있는 렌더러

## 주요 기능
1. UIGraphicsImageRenderer 인스턴스를 만들 때 출력 이미지의 크기와 포맷 객체를 함께 전달  
2. 포맷 객체를 생략하면 현재 디바이스 기준의 기본 설정이 사용됨
3. 원하는 출력 형식에 따라 적절한 렌더링 메서드를 선택
	- `image(actions:)`: `UIImage`를 반환
	- `jpegData(withCompressionQuality:actions:)`: JPEG 형식의 `Data`반환
	- `pngData(actions:)`: PNG 형식의 `Data` 반환
4. 선택한 메서드를 실행할 때, 클로저 안에 Core Graphics 기반의 드로잉 코드를 작성하여 이미지 내용 정의
5. 블렌드 모드 사용 등 고급 기능도 드로잉 코드 안에서 활용할 수 있으며, Core Graphics 함수도 함께 사용할 수 있음


## 코드 예시

### 기본 쉐이프 그리기

```swift
let renderer = UIGraphicsImageRenderer(size: .init(width: 200, height: 200))
let image = renderer.image { context in
	UIColor.darkGray.setStroke()
	context.stroke(renderer.format.bounds)
	UIColor(red: 158/255, green: 215/255, blue: 245/255, alpha: 1).setFill()
	context.fill(CGRect(x: 1, y: 1, width: 140, height: 140))
}
```

![[Pasted image 20250724105426.png]]

### 블렌드 모드 (겹치기)

```swift
let renderer = UIGraphicsImageRenderer(size: .init(width: 200, height: 200))
let image = renderer.image { context in
	UIColor.darkGray.setStroke()
	context.stroke(renderer.format.bounds)
	UIColor(red: 158/255, green: 215/255, blue: 245/255, alpha: 1).setFill()
	context.fill(CGRect(x: 1, y: 1, width: 140, height: 140))
	UIColor(red: 145/255, green: 211/255, blue: 205/255, alpha: 1).setFill()
	context.fill(CGRect(x: 60, y: 60, width: 140, height: 140), blendMode: .multiply)
}
```

![[Pasted image 20250724105511.png]]

### Core Grapics 직접 쓰기

```swift
let renderer = UIGraphicsImageRenderer(size: .init(width: 200, height: 200))
let image = renderer.image { context in
	UIColor.darkGray.setStroke()
	context.stroke(renderer.format.bounds)
	UIColor(red: 158/255, green: 215/255, blue: 245/255, alpha: 1).setFill()
	context.fill(CGRect(x: 1, y: 1, width: 140, height: 140))
	UIColor(red: 145/255, green: 211/255, blue: 205/255, alpha: 1).setFill()
	context.fill(CGRect(x: 60, y: 60, width: 140, height: 140), blendMode: .multiply)
	
	UIColor(red: 203/255, green: 222/255, blue: 116/255, alpha: 0.6).setFill()
	context.cgContext.fillEllipse(in: CGRect(x: 60, y: 60, width: 140, height: 140))
}
```

![[Pasted image 20250724105647.png]]

### 폰트 렌더링하기

```swift
let renderer = UIGraphicsImageRenderer(size: .init(width: 200, height: 200))
let image = renderer.image { context in
	UIColor.darkGray.setStroke()
	context.stroke(renderer.format.bounds)
	UIColor(red: 158/255, green: 215/255, blue: 245/255, alpha: 1).setFill()
	context.fill(CGRect(x: 1, y: 1, width: 140, height: 140))
	
	let text = "아우 하기 싫어"
	let attributes: [NSAttributedString.Key: Any] = [
		.font: UIFont.systemFont(ofSize: 24, weight: .bold),
		.foregroundColor: UIColor.black
	]
	
	let point = CGPoint(x: 20, y: 30)
	text.draw(at: point, withAttributes: attributes)
}
```

![[Pasted image 20250724105703.png]]

### 이미지 넣기

```swift
let renderer = UIGraphicsImageRenderer(size: .init(width: 200, height: 200))
return renderer.image { context in
	UIColor.darkGray.setStroke()
	context.stroke(renderer.format.bounds)
	
	// UIImage(named: "Chicken")?.draw(at: .init(x: 20, y: 20)) // 리사이징 안됨
	if let image = UIImage(named: "Chicken") {
		let scaledRect = CGRect(x: 20, y: 20, width: 160, height: 160)
		image.draw(in: scaledRect)
	}
	
	let text = "이제 할 일을 하자"
	let attributes: [NSAttributedString.Key: Any] = [
		.font: UIFont.systemFont(ofSize: 16, weight: .bold),
		.foregroundColor: UIColor.black
	]
	text.draw(at: CGPoint(x: 20, y: 160), withAttributes: attributes)
}
```

![[Pasted image 20250724105740.png]]

## Keywords
+ [[CoreGraphics]]

## References
- [UIKit - UIGraphicsImageRenderer](https://developer.apple.com/documentation/uikit/uigraphicsimagerenderer)
- [WWDC18 - iOS Memory Deep Dive](https://developer.apple.com/kr/videos/play/wwdc2018/416) 18분 쯤 이미지 그릴 때의 메모리 최적화 관련 이야기가 나온다. 아직 보지는 못했다.
