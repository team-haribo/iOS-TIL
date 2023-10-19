# Coordinator

<br>

앱의 화면 전환과 내비게이션 스택을 관리한다.

화면에 의존성을 주입하고 객체를 생성해 화면을 전환하는 역할을 한다.

<br><br><br>

## Coordinator 사용법

<br>

```swift
var window: UIWindow?
var appCoordinator: AppCoordinator?
```

AppDelegate 또는 SceneDelegate 파일에서 UIWindow를 설정하고 초기 Coordinator를 생성한다.

<br><br>

```swift
protocol Coordinator : class {
    var childCoordinators : [Coordinator] { get set }
		var navigationController: UINavigationController { get set }
    func start()
}

class AppCoordinator: Coordinator {
    
    var childCoordinators: [Coordinator] = []
    var navigationController: UINavigationController
    
    init(navigationController: UINavigationController) {
        self.navigationController = navigationController
    }
    
    func start() {}

		// 화면 전환 및 내비게이션 스택 관리
    func pushViewController(_ viewController: UIViewController, animated: Bool) {
        navigationController.pushViewController(viewController, animated: animated)
    }
    
    func popViewController(animated: Bool) {
        navigationController.popViewController(animated: animated)
    }
}
```

Coordinator 프로토콜을 정의하고 AppCoordinator를 구현한다.

- childCoordinators: 하위 Coordinator들을 저장하는 배열

- navigationController: 현재 Coordinator에서 사용할 Navigation Controller

- start: Coordinator의 시작점을 나타내는 메서드

<br><br>

```swift
class ACoordinator: Coordinator {
    func ADidFinish() {
        let BCoordinator = BCoordinator(navigationController: navigationController)
        BCoordinator.start()
    }
}
```

각 화면을 관리하는 Coordinator 클래스를 생성한다.

<br><br>

```swift
class AViewController: UIViewController {
    var coordinator: ACoordinator?
}
```

ViewController에서 해당 Coordinator를 참조하도록 한다.

<br><br><br>

## Coordinator 간 통신?

<br>

```swift
class BCoordinator: Coordinator {

    func start() {
        // 화면 표시
        let bViewController = BViewController()
        // 정보 전달
        bViewController.user = User(name: "ain", email: "ain@gmail.com")
        // 화면 전환
        pushViewController(BViewController, animated: true)
    }
}
```

ACoordinator가 작업을 완료하면 BCoordinator를 시작하게 된다.

BCoordinator에서는 사용자 정보를 전달하는 등 화면 간 데이터를 전달할 수 있다.

<br><br><br>

## MVVM-C ?

<br>

MVVM에서 뷰컨트롤러 계층을 관리하는 Coordinator를 따로 분리하는 것

<br>

```swift
// ViewModel 생성
let aViewModel = AViewModel()

// View 생성 및 ViewModel 연결
let aViewController = AViewController(viewModel: aViewModel)

// Coordinator와 View 연결
aViewController.coordinator = self 

// 화면 전환
navigationController.pushViewController(aViewController, animated: true)
```

Coordinator 클래스 내에서 ViewModel을 생성하고 View에 연결한다.

<br><br>

```swift
class AViewModel {
		//...
}

class AViewController: UIViewController {
    var viewModel: AViewModel?
    var coordinator: ACoordinator?

    init(viewModel: AViewModel) {
        self.viewModel = viewModel
        super.init(nibName: nil, bundle: nil)
    }
}
```

특정 ViewModel과 연결해 데이터를 전달한다.

위 코드에서 Coordinator는 ViewModel과 View를 연결하며, 화면 전환을 처리한다!

<br><br><br>

## Coordinator의 장단점

<br>

**장점**

- 각 화면, 모듈이 독립적으로 관리되므로 모듈화가 쉽고 재사용성이 높아진다.
- 내비게이션 로직이 중앙 집중화되므로 유지보수가 쉬워진다.

**단점**

- Coordinator 패턴을 도입하면 추가적인 코드가 필요하기 때문에 복잡해질 수 있다.