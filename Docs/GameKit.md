>[!question]
>GQ1. GameKit은 무엇인가?
>GQ2. GQ를 쓰세요

## Description
- 게임 센터를 이용해 소셜 게임 네트워크 기능을 구현할 때 필요한 기능을 제공하는 프레임워크
- [Apple Unity Plug-Ins](https://github.com/Apple/UnityPlugins)를 활용하면 Unity 게임과의 연동도 가능

## 주의할 점
- GameKit 클래스와 API를 호출하기 전에 반드시 앱에 GameCenter를 프로젝트에서 활성화하고 런타임에서 로컬 유저를 초기화해야 함. 그렇지 않으면 [`GKError.Code.notAuthenticated`](https://developer.apple.com/documentation/gamekit/gkerror/code/notauthenticated) 발생
- GameCenter가 iOS 26부터는 Games로 변경됨. API 수준에서 큰 변화는 없겠지만 GameCenter UI를 직접 사용하는 경우 룩앤필이 달라지는 문제는 불가피

## 주요 기능
+ 리더보드(GKLeaderboard)
	+ 일반 리더보드 뿐 아니라 주기적으로 초기화되는 리더보드(recurring leaderboards)도 만들 수 있음
+ 멀티플레이
	+ 실시간 멀티플레이 경험, 턴제 멀티플레이 경험을 위한 기능을 제공
- UI
	- 게임센터의 기본적인 UI를 제공
	- 데이터를 게임 안으로 끌어올 수도 있음
	- [HIG](https://developer.apple.com/design/human-interface-guidelines/game-center)

## 준비

### Game Center 앱에 활성화
1. 프로젝트 파일을 열고 Signing & Capabilities 탭을 연다
2. Capabilities에 Game Center를 추가한다
	![[Docs/Assets/Pasted image 20250717101154.png]]

### App Store Connect 준비
1. 현재 프로젝트의 Bunlde ID를 참고해 앱 레코드를 추가한다 [참고](https://developer.apple.com/help/app-store-connect/create-an-app-record/add-a-new-app/)
	![[Docs/Assets/Pasted image 20250717101044.png]]
2. 해당 앱 레코드에 들어가 좌측의 `성장 및 마케팅 -> Game Center`로 들어가면 GameCenter 컴포넌트를 추가할 수 있다. (리더보드, 업적 등)
	![[Pasted image 20250717101924.png]]
3. 이렇게 추가한 GameCenter 컴포넌트는 앱의 버전 별로 추가해줘야 한다. 앱을 한 번 App Store Connect에 업로드 한 후 해당 버전으로 들어간다.
4. 하단의 Game Center에서 Game Center 기능을 해당 버전에 활성화하고 어떤 컴포넌트를 추가할지 지정할 수 있다.
	![[Pasted image 20250717102101.png]]

## 로컬 플레이어 인증하기
- 매번 앱에서 GameKit을 찾혹

## Keywords
+ 파생된 키워드들을 작성
+ [[GKLeaderboard]]

## References
- [GameKit](https://developer.apple.com/documentation/gamekit)