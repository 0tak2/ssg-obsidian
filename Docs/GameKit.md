>[!question]
>GQ1. GameKit은 무엇인가?
>GQ2. GQ를 쓰세요

## Description
- 게임 센터를 이용해 소셜 게임 네트워크 기능을 구현할 때 필요한 기능을 제공하는 프레임워크
- [Apple Unity Plug-Ins](https://github.com/Apple/UnityPlugins)를 활용하면 Unity 게임과의 연동도 가능

## 주의할 점
- GameKit 클래스와 API를 호출하기 전에 반드시 앱에 GameCenter를 프로젝트에서 활성화하고 런타임에서 로컬 유저를 초기화해야 함. 그렇지 않으면 [`GKError.Code.notAuthenticated`](https://developer.apple.com/documentation/gamekit/gkerror/code/notauthenticated) 발생

## 주요 기능
+ 리더보드(GKLeaderboard)
	+ 일반 리더보드 뿐 아니라 주기적으로 초기화되는 리더보드(recurring leaderboards)도 만들 수 있음
+ 멀티플레이
	+ 실시간 멀티플레이 경험, 턴제 멀티플레이 경험을 위한 기능을 제공
- UI
	- 게임센터의 기본적인 UI를 제공
	- 데이터를 게임 안으로 끌어올 수도 있음
	- [HIG](https://developer.apple.com/design/human-interface-guidelines/game-center)

## Keywords
+ 파생된 키워드들을 작성
+ [[GKLeaderboard]]

## References
- [GameKit](https://developer.apple.com/documentation/gamekit)