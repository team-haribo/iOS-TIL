# UIKIT이란?
 #### Swift와 Objective-C와 같은 객체지향 프로그래밍 언어로 작성된 프레임워크입니다.
----
### SwiftUI와 UIkit의 차이점
#### 선언적 UI vs 명령적 UI
- SwiftUI: SwiftUI는 선언적 UI 프레임워크입니다. 코드로 UI 요소의 모양과 동작을 선언하며, 변경 사항이 자동으로 반영되도록 설계되었다 UI 요소의 상태와 레이아웃을 선언하고 변경 사항을 관리하기 위해 별도의 코드가 필요하지 않습니다.

- UIKit: UIKit은 명령적 UI 프레임워크입니다. UI 요소의 모양과 동작을 직접 코드로 지정하고, UI의 변경 및 상태 관리를 개발자가 수동으로 처리해야 합니다.
----
#### 레이아웃 방식
- SwiftUI: SwiftUI는 상대적인 레이아웃, 스택 뷰, 그리드 뷰 등과 같은 다양한 레이아웃 방식을 제공합니다.

- UIKit: UIKit은 ``UIView``, ``UIViewController`` 및 ``Auto Layout``과 함께 복잡한 레이아웃을 구성합니다.
----
#### 동적 UI
- SwiftUI: SwiftUI는 Swift 언어의 강력한 특성을 활용하여 모던하고 간결한 API를 제공합니다.

- UIKit: UIKit은 ``Objective-C``와 C 언어 기반으로 구현되어 있으며, ``API``가 좀 더 번거롭고 명령적입니다.
----
#### State / Data Flow
- SwiftUI: SwiftUI는 ``@State``, ``@Binding``, ``@ObservedObject`` 등의 속성 래퍼를 사용하여 UI 상태 및 데이터 흐름을 관리합니다. 상태 변경 시 UI가 자동으로 업데이트됩니다.

- UIKit: UIKit에서는 상태 및 데이터 흐름 관리를 수동으로 처리해야 하며, ``NSNotification``, ``KVO``, 델리게이트 패턴 등을 사용하여 상호작용합니다.


------

### 코드로 보는 대표적인 차이점
```swift
import SwiftUI

struct ContentView: View {
    @State private var isToggled = false
    
    var body: some View {
        VStack {
            Text("Hello, SwiftUI!")
                .font(.title)
                .foregroundColor(isToggled ? .blue : .green)
            
            Button(action: {
                isToggled.toggle()
            }) {
                Text("Toggle Text Color")
            }
        }
    }
}

@main
struct jiwanSwiftui: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```


```swift
import UIKit

class ViewController: UIViewController {
    var label: UILabel!
    var button: UIButton!
    var isToggled = false
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        label = UILabel()
        label.text = "Hello, UIKit!"
        label.font = UIFont.boldSystemFont(ofSize: 24)
        label.textColor = .green
        label.translatesAutoresizingMaskIntoConstraints = false
        
        button = UIButton(type: .system)
        button.setTitle("Toggle Text Color", for: .normal)
        button.addTarget(self, action: #selector(toggleTextColor), for: .touchUpInside)
        button.translatesAutoresizingMaskIntoConstraints = false
        
        view.addSubview(label)
        view.addSubview(button)
        
        NSLayoutConstraint.activate([
            label.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            label.centerYAnchor.constraint(equalTo: view.centerYAnchor),
            
            button.topAnchor.constraint(equalTo: label.bottomAnchor, constant: 20),
            button.centerXAnchor.constraint(equalTo: view.centerXAnchor),
        ])
    }
    
    @objc func toggleTextColor() {
        isToggled.toggle()
        label.textColor = isToggled ? .blue : .green
    }
}

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        window = UIWindow(frame: UIScreen.main.bounds)
        let viewController = ViewController()
        window?.rootViewController = viewController
        window?.makeKeyAndVisible()
        return true
    }
}

```
- 두개다 비슷한 기능을 수행하지만 선언적 UI와 명령적 UI의 차이가 있습니다.
따라서 기능적으론 동일하지만 접근 방식이 다르며, SwiftUI가 훨씬 간결한 코드를 작성할 수 있습니다.