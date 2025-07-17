>[!question]
>GQ1. GQ를 쓰세요
>GQ2. GQ를 쓰세요

## Description
- 게임 센터에는 한 번에 한 명의 유저가 로그인할 수 있고, 그게 로컬 플레이어이다.
- GameKit의 대부분 기능은 로컬 유저에 의존하므로, GameKit 기능을 사용하기 전에 로컬 플레이어가 게임센터에 로그인되어있는지 확인해야 한다.
- 따라서 GameKit과 관련된 기능을 사용하기 전에 로컬 유저를 초기화해야하며, 이때 가장 중요한 작업이 authenticateHandler를 작성하는 것이다.
- 상속 구조: GKBasePlayer <- GKPlayer <- GKLocalPlayer
	- GKPlayer는 현재 플레이어가 아닌 리모트의 다른 플레이어를 표현하는 타입

## 주요 기능

### 유저 인증
- authenticateHandler 구현
	```swift
	func authenticatePlayer() {
		let localPlayer = GKLocalPlayer.local
		localPlayer.authenticateHandler = { viewController, error in
			if let viewController = viewController {
				// TODO: UI에서 ViewController를 띄워야 함
			} else if localPlayer.isAuthenticated {
				DispatchQueue.main.async {
					// TODO: 인증 성공에 따른 처리 (닉네임 표시, 대시보드 버튼 노출 등)
					self.isAuthenticated = true
					self.playerName = localPlayer.displayName
				}
			} else {
				// TODO: 인증 실패에 따른 처리 (GameKit 관련 기능 숨기기 등)
				print("Game Center 인증 실패: \(error?.localizedDescription ?? "알 수 없는 오류")")
			}
		}
	}
	```
- GameKit은 아래와 같은 상황에서 이 핸들러를 호출하며, 상황 별로 대응이 필요하다.
	- If the local player needs to perform some action, GameKit passes a view controller that you must present to the player to complete initialization.
	- If the player successfully signs in, GameKit sets the local player’s [`isAuthenticated`](https://developer.apple.com/documentation/gamekit/gklocalplayer/isauthenticated) property to [`true`](https://developer.apple.com/documentation/swift/true) and calls the handler again, this time passing `nil` for both the view controller and error parameters. You can then start the game.
	- If the player decides not to sign in or create a Game Center account, GameKit sets the local player’s [`isAuthenticated`](https://developer.apple.com/documentation/gamekit/gklocalplayer/isauthenticated) property to [`false`](https://developer.apple.com/documentation/swift/false) and calls the handler again by passing an error that indicates the reason the player isn’t available. In this case, disable Game Center in your game.
	- If the local player previously signed in on the device when you set the handler, GameKit sets the local player’s [`isAuthenticated`](https://developer.apple.com/documentation/gamekit/gklocalplayer/isauthenticated) property to [`true`](https://developer.apple.com/documentation/swift/true) and passes `nil` for both the view controller and error parameters, and you can start the game.
- 다양한 제약 확인하기
	```swift
	GKLocalPlayer.local.authenticateHandler = { viewController, error in
	    if let viewController = viewController {
	        // Present the view controller so the player can sign in.
	        return
	    }
	    if error != nil {
	        // Player is not available
	        // Disable Game Center in the game.
	        return        
	    }
	    
	    // Player is available.
	    // Check if there are any player restrictions before starting the game.
	            
	    if GKLocalPlayer.local.isUnderage {
	        // Hide explicit game content.
	    }
	
	
	    if GKLocalPlayer.local.isMultiplayerGamingRestricted {
	        // Disable multiplayer game features.
	    } 
	
	
	    if GKLocalPlayer.local.isPersonalizedCommunicationRestricted {
	        // Disable in game communication UI.
	    }
	    
	    // Perform any other configurations as needed (for example, access point).
	}
	```

## Keywords
+ 파생된 키워드들을 작성

## References
- [GameKit - GKLocalPlayer](https://developer.apple.com/documentation/gamekit/gklocalplayer)
- [Authenticating a player](https://developer.apple.com/documentation/gamekit/authenticating-a-player)
