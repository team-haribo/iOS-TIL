# RxFlow

<br>

Coordinator 패턴을 RxSwift와 함께 사용할 수 있도록 랩핑한 프레임워크

<br><br><br>

## **Coordinator패턴과의 차이**

<br>

- RxFlow에서는 화면을 **State**로, 화면 전환을 **Concept**로 정의한다.

- Coordinator 패턴은 주로 동기적인 화면 전환을 처리한다면 RxFlow는 비동기적인 화면 전환을 처리한다.

<br><br><br>

## RxFlow 사용법

<br>

```swift
enum AppStep: Step {
    case firstScreen
    case secondScreen
}
```

RxFlow를 프로젝트에 추가하고 State와 Concept를 정의한다.

<br>

```swift
// FirstViewController
class FirstViewController: UIViewController, Stepper {
    var steps = PublishRelay<Step>()
    @IBAction func goToSecondScreenButtonTapped() {
        steps.accept(AppStep.secondScreen)
    }
}

// SecondViewController
class SecondViewController: UIViewController, Stepper {
    var steps = PublishRelay<Step>()
}
```

화면 전환에 사용되는 FirstViewController, SecondViewController

<br><br>

```swift
class AppFlowCoordinator: FlowCoordinator {
    var root: Presentable {
        return self.navigationController
    }

    let navigationController = UINavigationController()

    func navigate(to step: Step) -> FlowContributors {
        guard let step = step as? AppStep else { return .none }
        switch step {
            case .firstScreen:
                let firstViewController = FirstViewController()
                firstViewController.navigationItem.rightBarButtonItem = UIBarButtonItem(title: "Next", style: .plain, target: self, action: #selector(goToSecondScreen))
                navigationController.viewControllers = [firstViewController]
                return .one(flowContributor: .contribute(withNextPresentable: firstViewController, withNextStepper: firstViewController))
            case .secondScreen:
                let secondViewController = SecondViewController()
                navigationController.pushViewController(secondViewController, animated: true)
                return .none
        }
    }

    @objc private func goToSecondScreen() {
        steps.accept(AppStep.secondScreen)
    }
}

```

RxFlowCoordinator를 구현하여 각 Concept에 대한 화면 전환 로직을 처리한다.

위 코드는 첫번째 화면의 Next버튼을 누르면 두번째 화면으로 전환되는 코드이다 !

<br><br>

```swift
let appFlow = AppFlowCoordinator()
let appStepper = OneStepper(withSingleStep: AppStep.firstScreen)

Flows.whenReady(flow1: appFlow, block: { [unowned self] root in
    self.window.rootViewController = root
    self.window.makeKeyAndVisible()
})

self.coordinator.coordinate(flow: appFlow, with: appStepper)
```

AppDelegate에 RxFlow를 초기화하고 실행하는 코드를 작성한다.

<br><br>

- **Flow**: 어플리케이션 내부의 네비게이션 공간
    
    화면 전환을 관리하며, 각 Flow 내에서 화면 전환 스택을 관리한다.
    
- **Step**: 어플리케이션 내부의 네비게이션 상태
  
- **Stepper**: 사용자의 입력 또는 이벤트를 통해 화면 전환을 시작하는 역할
  
- **Presentable**: Flow 내부에서 화면을 나타내는데 사용되는 프로토콜
    
    UIViewController나 UINavigationController를 채택하며, Flow 내에서 화면을 나타내는 역할을 한다.
    
- **NextFlowItem**: 현재 화면 전환 이후에 다음에 어떤 화면이 나타날지를 정의하는 객체
    
    Step과 함께 사용되며 다음 화면을 결정하는 역할을 한다.

<br><br><br>
    

## RxFlow의 장점

<br>

- Coordinator 패턴을 쉽게 적용할 수 있다.

- 앱의 화면 전환을 효과적으로 관리할 수 있다.