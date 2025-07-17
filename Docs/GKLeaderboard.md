>[!question]
>GQ1. 내 앱에 GameKit으로 리더보드를 추가하려면 어떻게 해야할까?
>GQ2. GameKit 리더보드에 점수를 추가하려면 어떻게 해야할까?

## Description
- Game Center가 저장하고 있는 리더보드를 표현하는 타입

## 주요 기능
- 클래식 리더보드(새로 초기화되지 않음)에 점수 저장, 랭킹 불러오기
- Recurring 리더보드(주기적으로 초기화됨)에 점수 저장, 랭킹 불러오기

## 리더보드 만들기
- Xcode에서 프로젝트에 GameKit File을 추가한다.
	![[Pasted image 20250717111551.png]]
- 편집기가 열리는데, 왼쪽 컬럼의 하단 + 버튼을 눌러 리더보드를 추가한다.
	![[Pasted image 20250717111618.png]]
- 이름, ID, 형식을 지정할 수 있다.
	![[Pasted image 20250717111639.png]]
- 완료했으면 App Store Connect에 Push한다.
	![[Pasted image 20250717111748.png]]
- 현지화를 적용해야 인게임 대시보드에서 리더보드 이름이 잘 표시된다.
	![[Pasted image 20250717112125.png]]

## 코드 예시
+ 리더보드에 점수 추가
	- 특정한 리더보드 인스턴스에 점수 추가: [`submitScore(_:context:player:completionHandler:)` 또는 `submitScore(_:context:player:) async`](https://developer.apple.com/documentation/gamekit/gkleaderboard/submitscore(_:context:player:completionhandler:)) (인스턴스 메서드)
	- 한 개 이상의 리더보드에 리더보드 아이디를 명시하여 점수 추가: [`submitScore(_:context:player:leaderboardIDs:completionHandler:)` 또는 `submitScore(_:context:player:leaderboardIDs) async`](https://developer.apple.com/documentation/gamekit/gkleaderboard/submitscore(_:context:player:leaderboardids:completionhandler:)) (타입 메서드)

	```swift
	    func submitRecord() {
	        guard let elapsedTime = self.lastGameResult?.elapsedTime,
	           let lastGameLevel = self.lastGameResult?.level else {
	            print("❌ No last result")
	            return
	        }
	        
	        Task.detached {
	            do {
	                try await GKLeaderboard.submitScore(Int(elapsedTime),
	                                                    context: 0,
	                                                    player: GKLocalPlayer.local,
	                                                    leaderboardIDs: [lastGameLevel.leaderboardId])
	                print("score submitted")
	            } catch {
	                print("error during submitting score: \(error)")
	            }
	        }
	    }
	```

- 리더보드 읽기
	- 한 개 이상의 리더보드를 아이디를 통해서 불러오려면 [`loadLeaderboards(IDs:completionHandler:)`](https://developer.apple.com/documentation/gamekit/gkleaderboard/loadleaderboards\(ids:completionhandler:\))를 사용한다.
	- 특정 리더보드 인스턴스에서 점수를 가져오려면 [`loadEntries(for:timeScope:range:completionHandler:)`](https://developer.apple.com/documentation/gamekit/gkleaderboard/loadentries\(for:timescope:range:completionhandler:\))를 사용한다.

```swift

```

## Keywords
+ 파생된 키워드들을 작성

## References
- [GKLeaderboard](https://developer.apple.com/documentation/gamekit/gkleaderboard)
- [Encourage progress and competition with leaderboards](https://developer.apple.com/documentation/gamekit/encourage-progress-and-competition-with-leaderboards)
- 여기에서는 다루지 않았지만 Recurring Leaderboard에 대해서는 [여기](https://developer.apple.com/documentation/gamekit/creating-recurring-leaderboards)를 참고