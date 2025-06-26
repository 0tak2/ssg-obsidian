>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- AR(라이브 뷰에 2D, 3D 그래픽을 결합)을 위해 필요한 기능을 제공하는 프레임워크
- 공간, 물체를 인식하고 트래킹하는 기능, 그 외 편의 기능 제공
- 이렇게 인식한 공간이나 물체에 대해 2D, 3D 모델을 렌더링하려면 [[SpriteKit]], [[SceneKit]], [[RealityKit]] 등이 필요함

## AR 매커니즘
**1. Visual-Inertial Odometry (VIO)**
- 카메라 영상과 가속도계, 자이로스코프 데이터를 결합해 위치 추정
- GPS 없이도 실내에서 정밀한 위치 및 방향 추적 가능
- 가상 객체가 현실 세계에 안정적으로 고정되도록 보장

**2. 행렬 변환 및 공간 매핑 (Matrix Transformations & Spatial Mapping)**
- 3D 공간 내에서 기기의 위치와 방향(자세, pose)을 계산
- 좌표계를 변환해 가상 객체의 위치를 실제 세계와 일치시킴
- 실시간으로 주변 환경의 기하학적 구조를 파악하여 정밀한 매핑 수행
    
**3. TrueDepth 및 후면 카메라**
- **TrueDepth**: 적외선 센서로 얼굴의 깊이 정보와 표정을 정밀하게 캡처
- **후면 카메라**: 영상 프레임과 모션 데이터를 결합해 주변 환경을 스캔
- 두 카메라의 조합으로 사람과 공간 모두에 대한 정밀한 인식 가능

## 주요 기능

### 세션
- [ARSession](https://developer.apple.com/documentation/arkit/arsession)
- The object that manages the major tasks associated with every AR experience, such as motion tracking, camera passthrough, and image analysis.

### 설정
- [ARConfiguration](https://developer.apple.com/documentation/arkit/arconfiguration)
- 하위 타입
	- [`ARBodyTrackingConfiguration`](https://developer.apple.com/documentation/arkit/arbodytrackingconfiguration)
	- [`ARFaceTrackingConfiguration`](https://developer.apple.com/documentation/arkit/arfacetrackingconfiguration)
	- [`ARGeoTrackingConfiguration`](https://developer.apple.com/documentation/arkit/argeotrackingconfiguration)
	- [`ARImageTrackingConfiguration`](https://developer.apple.com/documentation/arkit/arimagetrackingconfiguration)
	- [`ARObjectScanningConfiguration`](https://developer.apple.com/documentation/arkit/arobjectscanningconfiguration)
	- [`AROrientationTrackingConfiguration`](https://developer.apple.com/documentation/arkit/arorientationtrackingconfiguration)
	- [`ARPositionalTrackingConfiguration`](https://developer.apple.com/documentation/arkit/arpositionaltrackingconfiguration)
	- [`ARWorldTrackingConfiguration`](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration)

```swift
let configuration = ARWorldTrackingConfiguration() // ARConfiguration의 하위 타입 중 WorldTracking 선택 - GQ: 다른 트래킹 방법에는 뭐가 있지?
sceneView.session.run(configuration) // MARK: run a ARKit session
```

### feature points
- https://developer.apple.com/documentation/arkit/arframe/rawfeaturepoints
- A point automatically identified by ARKit as part of a continuous surface, but without a corresponding anchor.
- 실제 세상에서의 좌표와 매핑되는 좌표로, ARKit에서 인식한 것

### hitTest와 raycast
- 두 방법 모두 AR 환경과 사용자 인터랙션을 구현하는데 필요. 다만 목적과 특성이 다름.
- AR에서 **Raycast**는 가상의 **광선(ray)**을 3D 공간에 쏘아 물체, 표면, 또는 특정 지점과의 **충돌 여부(hit test)**를 판별하는 **기초 공간 인식 기법**

- 목적
	- hitTest: 2D 화면과 가상 객체 및 피쳐 포인트가 겹치는 부분을 찾을 때 사용. 이미 씬에 렌더링 되고 있는 객체와 인터랙션을 구현할 때 보통 사용.
	- raycast: 2D 화면과 ARKit이 인식한 실제 세계의 표면(surface) 간의 겹치는 부분을 찾을 떄 사용. 새로운 객체를 놓을 때 주로 사용.
- 정확도, 효율성
	- hitTest: 항상 정확한 결과를 보장하지는 않음. 특히 표면 검출에 대해서 떨어짐.
	- raycast: 나중에 나온 기능. ARKit과 더 잘 통합되어있고 알고리즘이 개선되어 실제 세계 평면 검출에 있어 높은 정확도를 보임.
- 피쳐 포인트
	- hitTest: 피쳐 포인트를 알아낼 수 있지만, 표면에 대한 자세한 정보를 가져오거나 활용하고 싶을 때에는 불편함
	- raycast: 개별적인 피쳐 포인트보다, 바닥이나 책상같은 평면 검출에 더 집중되어 있음. 더 넓은 표면 영역에 적합함

## 코드 예시
- https://github.com/0tak2/ios-study/blob/main/self-study/ARKitPractice/ARKitPractice/ARContainerViewController.swift

## Keywords
+ 파생된 키워드들을 작성

## References
- 참고한 레퍼런스를 작성 (예 : Apple의 공식 문서)

