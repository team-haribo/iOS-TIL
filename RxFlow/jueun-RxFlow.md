## RxFlow

### RxFlow란?
- Reactive Flow Coordinator패턴을 기반으로 하는 iOS 네비게이션 프레임워크
- Coordinator패턴을 RxSwift에 녹여낸 라이브러리

<br>

### RxFlow의 장점
- 스토리보드를 유닛단위로 분리하여 UIViewController의 재사용성을 키움
- 네비게시연의 흐름(context)에 맞게 UIViewController를 다른 방식으로 보여줄 수 있음
- 의존성 주입(Dependency Injection)을 쉽게 구현할 수 있음
- UIViewController에 있는 모든 네비게이션 매커니즘을 삭제함
- 반응형 프로그래밍(Reactive Programming) 사용르 촉진함
- 네비게이션에서 일어나는 대부분의 케이스를 처리하면서 선언형으로 표현할 수 있음
- 어플리케이션을 네비게이션의 논리적인 블록으로 나눌 수 있음

<br>

### RxFlow의 6가지 특징
1. Flow : 네비게이션 작업(ViewController or 다른 Flow 표시)를 선언
2. Step : Flow화면에서 선언된 화면으로 전파될 내부값(Id, URL)을 포함할 수 있음
3. Stepper : Flow의 모든 네비게이션액션을 emit
4. Presentable : 표시될 수 있는 추상화타입(기본적으로 UIViewController 및 Flow는 표시 가능)
5. FlowContributor : FlowCoordinator에게 Flow의 새 단계를 생성할 수 있는 다음 항목이 무엇인지 알려주는 데이터 구조
6. FlowCoordinator

<br>

### 정의
1. Step 정의
   
    : Step은 네비게이션 의도를 표현하는 각각의 상태 / enum현태로 선언하는 것이 편리 / 각각의 상태에 따른 case 정의
2. Flow 정의

    : Flow는 ViewController를 인스턴스화 할 때 의존성 주입(DI)을 구현하는데 사용
3. Stepper 정의

    : Step을 생성할 수 있는 모든 것, Stepper는 Flow내 모든 네비게이션 액션을 트리거 (ViewModel에서 분리시키는 것이 좋음)
4. 딥링킹(Deep Linking) 사용

    : AppDelegate에서 FlowCoordinator에 도달하고 예를 들어 알림을 수신할 때 navigate(to:) 함수를 호출하여 사용할 수 있음