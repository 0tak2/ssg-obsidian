>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- 개요와 설명을 작성

## 주요 기능

### 유저 인증
- GameKit의 대부분 기능은 로컬 유저에 의존한다. 따라서 GameKit 관련 기능이 시작되기 전 로컬 유저를 인증해두는 것이 권장된다.
- 
	```swift
	func authenticatePlayer() {
		let localPlayer = GKLocalPlayer.local
		localPlayer.authenticateHandler = { viewController, error in
			if let viewController = viewController {
				// TODO: UI에서 ViewController를 띄워야 함
			} else if localPlayer.isAuthenticated {
				DispatchQueue.main.async {
					self.isAuthenticated = true // TODO: 인증 성공에 따른 분기
					self.playerName = localPlayer.displayName
				}
			} else {
				print("Game Center 인증 실패: \(error?.localizedDescription ?? "알 수 없는 오류")")
				// TODO: 인증 실패에 따른 
			}
		}
	}
	```
- 

## 코드 예시
+ 실제 코드 예시를 작성

## Keywords
+ 파생된 키워드들을 작성

## References
- [GameKit - GKLocalPlayer](https://developer.apple.com/documentation/gamekit/gklocalplayer)
- [Authenticating a player](https://developer.apple.com/documentation/gamekit/authenticating-a-player)
