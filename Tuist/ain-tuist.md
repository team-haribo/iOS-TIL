# Tuist

<br>

XcodeGen 과 유사하게 Xcode 프로젝트를 생성/관리할 수 있도록 도와주는 도구

<br>

XcodeGen은 yml 이나 json 으로 프로젝트 설정을 관리하지만

Tuist 는 Project.swift 라는 **Swift 파일로 설정을 관리**한다는 차이가 있다.

<br><br><br>

## Tuist를 사용한다면 ?

<br>

Tuist는 Project.swift 파일을 통해 의존성 및 타겟, 프로젝트를 설정하는 방식으로

Xcode 프로젝트와 워크스페이스 파일을 생성하지 않고

프로젝트 설정 및 소스 코드 디렉토리를 다시 정의하는 것에 중점을 두어 

프로젝트 파일 관리의 **복잡성을 줄이고 git 충돌 가능성을 감소**시킨다 !

<br><br><br>

## Tuist 사용법

<br>

**Tuist 설치하기**

```jsx
$ bash <(curl -Ls https://install.tuist.io)
```

<br>

**tuist 프로젝트 생성하기**

```jsx
$ tuist init --platform ios
```

원하는 폴더 경로에서 tuist init
( 자동으로 생성되는 예제 파일은 삭세해준다. )

<br>

****Project.swift 파일 설정하기****

💡 Project.swift는 프로젝트를 정의하는 파일!  

```jsx
tuist edit
```

위 명령어를 사용해서 Project.swift 파일을 열어주고 기본 설정을 진행한다.

<br>
  
Project.swift 설정 코드

```swift
public init(

	name: String, // 프로젝트 이름

	organizationName: String? = nil, // organization 이름

	options: ProjectDescription.Project.Options = .options(),

  // 프로젝트에 추가할 Swift Package
	packages: [
        .package(url: "https://github.com/Alamofire/Alamofire.git", .branch("master"))
  ],

	settings: ProjectDescription.Settings? = nil,

  // 프로젝트의 타겟
	targets: [
        Target(name: "StaticFrameworkKit", // 타겟 이름
               platform: .iOS, // 플랫폼
               product: .staticFramework, // Static Framework, Dynamic Framework, Static Library, App 등을 설정
               bundleId: "kr.minsone.StaticFrameworkKit", // BundleId
               deploymentTarget: .iOS(targetVersion: "13.0", devices: [.iphone, .ipad]), // 배포타겟 정보
               infoPlist: .default, // default는 plist를 생성해서 사용, plist를 경로를 적용하여 임의의 plist를 사용 가능
               sources: ["Sources/**"], // 소스 경로
               resources: [], // 리소스 경로
               actions: [], // 빌드시 실행할 Action
               dependencies: [
                // 타겟의 의존성 설정, Cocoapods, Carthage, XCFramework, Swift Package 등
                .package(product: "Alamofire")
               ]),
  ],

	schemes: [ProjectDescription.Scheme] = [], // 타겟에 해당하는 schemes

	additionalFiles: [ProjectDescription.FileElement] = []
)
```


<br>



****프로젝트 생성****

```jsx
tuist generate
```

설정을 마쳤다면 위 명령어로 프로젝트를 생성해주기 - !!!!!!

<br><br><br>

## ProjectDescription

<br>

Tuist 프로젝트의 핵심 구성 요소를 정의한다.

앞서 설명한 Project.swift 파일에 정의된 코드이다!

프로젝트의 폴더 구조, 타겟 및 의존성을 설정한다.

<br><br><br>

## Plugin


<br>

Tuist의 기능을 확장시키는 것

프로젝트의 빌드 프로세스나 배포 스크립트에 특정 기능을 추가하기 위한 요소

<br>

```swift
let plugin = Plugin(name: "MyPlugin") { plugin in
    plugin.targets = [PluginTarget(name: "MyPlugin", platform: .iOS)]
    plugin.actions = [PluginAction(name: "customAction", isEnabled: true, target: "MyApp")]
}
```

MyPlugin이라는 플러그인을 정의하고 

customAction이라는 사용자 정의 동작을 추가!
