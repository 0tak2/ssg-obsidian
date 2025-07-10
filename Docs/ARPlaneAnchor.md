>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- ARkit이 물리 세계에서 감지한 2D 평면에 대한 앵커
	An anchor for a 2D planar surface that ARKit detects in the physical environment.
- ARSession을 World Tracking 되도록 설정했을 때, planeDetection이 활성화되어있다면 

## 주요 기능
+ transform [`simd_float4x4`](https://developer.apple.com/documentation/simd/simd_float4x4)
	+ 앵커가 배치된 AR 세션의 월드 좌표 공간에 대한 앵커의 위치, 방향, 배율을 인코딩하는 행렬
	+ 뭔말일까? https://chatgpt.com/share/686f24ab-ecb0-800a-9cb1-d7e6704c27da

|구성|의미|
|---|---|
|상위 3x3 행렬 (`rotation + scale`)|회전 (보통 정규 직교 행렬이므로 단위 벡터 기준 회전만 표현)|
|마지막 열의 상위 3개 값 (`column.3.xyz`)|위치 (position, translation)|
|마지막 행|보통 `(0, 0, 0, 1)`|

- extent
	- Deprecated. planeExtent를 대신 사용.
		- 그렇지만 많은 예제가 아직 이 프로퍼티를 사용하고 있음.
	- 

## 코드 예시
+ 실제 코드 예시를 작성

## Keywords
+ 파생된 키워드들을 작성

## References
- https://developer.apple.com/documentation/arkit/arplaneanchor