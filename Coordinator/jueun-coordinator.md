## Coordinator

### Coordinator란?
> A Coordinator is an object that bosses or move view controllers around. Taking all of the driving logic out of your view controllers, and moving that stuff on layer up is gonna make your life a lot maore aweome.
>
> 
< Soroush Khanlou >

 : 하나 이상의 뷰 컨트롤러들에게 지시를 내리는 객체이며 여기서 말하는 지시는 view의 트랜지션을 의미 즉, Coordinator는 앱 전반에 있어 화면 전환 및 계층에 대한 흐름을 제어하는 역할을 함

 <br>

 ### Coordinator 패턴을 사용했을 때 장점

기존에는 화면전환을 ViewController가 담당함

때문에 부모 VC는 자식 VC를 알아야 했고, 자식 VC에 필요한 객체들 또한, 모두 부모 VC 낸부에서 인스턴스를 생성해야 했음 때문에 객체들 간 결합도가 높아질 수 밖에 업고, 아키텍쳐를 설계할 때 높은 결합 또는 유지 보수에 불리함

여기서 Coordinator 패텬을 사용한다면 더 이상 부모 VC는 자식 VC를 알 필요가 없음

자식 VC의 의존성을 주입하고 객체를 생성해 화면을 전환하는 역할은 Coordinator가 하기 때문 이제 VC는 자기 Coordinator에게 자식 VC로 화면전환을 요청하면 됨

→ 이제 VC는 오직 데이터를 화면에 뿌려주는 역할만 집중할 수 있게 되고, 때문에 유지 보수가 용이해짐

또한 Coordinator를 통한 화면 전환시 ViewController에서 사용할 ViewModel을 함께 주입해줄 수 있어 DI 또한 쉽게 해결할 수 있음

화면들(ViewControllers)의 계층을 관리, 화면간의 연결을 해결함

<br>

### Router

: Router는 실제로 ViewController를 present하고, dismiss 해주는 역할을 수행

<br>

### Coordinator

- ViewController 객체 생성

- ViewController가 present되는 순서(flow)를 결정

<br>

### Coordinator 프로토콜

```swift
protocol Coordinator {
	func start()
}
```

Coordinator 패턴의 가장 기본적인 프로토콜 형태는 start() 메서드임

각각의 Coordinator는 화면 presentation과 관련된 메서드를 가지고 있음

<br>

### Concrete Coordinator(Coordinator 구상체)

Coordinator 구상체는 당연히 Coordinator 프로토콜을 채택함

이 객체에서는 어떻게 다른 ViewController를 생성하는지, 그리고 이 다음에 무슨 ViewController가 나와야 하는지 알고 있음

<br>

### Router 프로토콜

Router 프로토콜은 당연한 이야기지만 Router 구상체들이 가져야 할 메서드들을 정의함

present, dismiss를 통해 다른 ViewController를 직접 보여주거나 없애는 기능을 가짐

<br>

### Concrete Router(Router 구상체)

Router 구상체는 실제로 어떻게 ViewController를 present 할 지 알고 있지만 어떤 ViewController가 다음에 나올지는 모르고 있음

Coordinator가 Router에게 어떤 ViewController를 불러야 하는지 알려주는 구조

<br>

### Concrete ViewController

위의 구조라면 당연히 하나의 ViewControlle는 다른 ViewController의 존재에 대해 모르게 됨

그리고 화면 전환은 Coordinator에게 위임하는 구조임

<br>

### MVVM-C
: MVVM에서 ViewController 계층을 관리하는 Coordinator를 따로 분리하는 것

<br>

### Coordinator 사용법
1. 자식 Coordinator를 사용하지 않는 방법

	1) Coordinator 프로토콜 생성
	2) 객체 생성하기
		- 많은 ViewController에서 공유되기 때문에 클래스로 작성
	3) 엔트리포인트에 Coordinator 연결하기
		- AppDelegate
		```Swift
		class AppDelegate: UIResponder, UIApplicationDelegate {
			// 생략

			func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
				let navController = UINavigationController()
				coordinator = MainCoordinator(navigationController: navController)
				coordinator?.start()
			
				window = UIWindow(frame: UIScreen.main.bounds)
				window?.rootViewController = navController
				window?.makeKeyAndVisible()

					return true
		}

		// 생략
		```
	- SceneDelegate
		```Swift
		class SceneDelegate: UIResponder, UIWindowSceneDelegate {
			// 생략

			func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }
        let navController = UINavigationController()
        
        coordinator = MainCoordinator(navigationController: navController)
        coordinator?.start()
        
        window = UIWindow(windowScene: windowScene)
        window?.rootViewController = navController
        window?.makeKeyAndVisible()
    		}
		}
		```
	4) 화면전환
	
<br>

2. 자식 Coordinator를 함께 사용하는 방법
	: AppDelegate(SceneDelegate)는 최상위 AppCoordinator를 유지, 모든 Coordinator에는 일련의 하위 Coordinator가 있음
   1. Coordinator 프로토콜 생성
   	- 자식 Coordinator들을 관리할 배열 선언
	2. 작업을 마친 경우 작동할 프로토콜을 선언
	3. 각각의 Coordinator의 Type을 지정할 enum을 선언
	4. 각각의 Coordinator의 프로토콜을 선언
	5. 객체(Coordinator)를 작성
  