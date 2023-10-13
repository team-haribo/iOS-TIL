## Tuist

<br>

### 😎 Tuist란?

: Xcode 프로젝트를 Swift로 생성/관리할 수 있도록 도와주는 도구 / Tuist를 사용하게 되면 Tuist가 `Project.swift`파일을 기반으로 프로젝트(.xcodeproj)를 생성해 주기 때문에 git conflict같은 불편을 겪지 않아도 됨

<br>

### 🤗 알아야 할 개념

- **Target**
    - project나 Workspace의 파일들을 빌드하여 생성되는 End Product를 의미
- Project
    - 모든 파일, 리소스를 빌드하는데 필요한 정보의 저장소(repository)
    - 프로젝트는 빌드하는 방법을 명시하는 end product인 target을 하나 이상 포함함
- Project가 가지고 있는 정보
    - 소스파일에 대한 참조
    - Structure navigator에서 소스파일을 그룹화
    - Debug, release와 같은 build configuration을 설정 가능
- .xcodeproj라는 디렉토리에 터미널을 통해서 들어가면 이런 정보가 존재
    - Project.pbxproj
        - 각 파일들의 참조값을 UUID들로 정의되어 있는 파일
- Workspace
    - Xcode의 Project 및 기타 리소스를 그룹화하여 합께 작업할 수 있는 Xcode document
    - 다수의 Project를 사용하고 싶은 경우, workspace 하위로 관리
    - 각 Project의 파일을 구성하는 것 외에도 workspace에 포함된 Project들과 Target간의 관계 제공

<br>

### 🤔 왜 Tuist를 사용할까? (장점)

- pbxproj 충돌을 줄일 수 있음
- Swift로 프로젝트 설정이 가능함
- 모듈화(Modularization)를 쉽게 하도록 도와줌
- XcodeGen과 다르게 Swift로 프로젝트 설정이 가능함
    - XcodeGen은 yaml 혹은 json으로 프로젝트 설정 파일을 만들어야 함
    
<br>

### 😚 모듈화를 적용했을 시 기대되는 효과

- 코드 안정성
- 개발 생산성
- 빌드 속도
- 사용 목적에 맞는 코드 배치

<br>

### 🫠 Tuist 사용해보기

> **Tuist 설치**
> 

```swift
curl -Ls https://install.tuist.io | bash
```

> **Tuist로 iOS 프로젝트 생성**
> 

```swift
tuist init --platform ios // UIKit
tuist init --platform ios --template swiftui // SwiftUI
```

4개의 파일이 생성됨

> **Project.swift를 수정하는 명령어 Tuist Edit 실행**
> 

```swift
tuist edit
```

<br>

### Project.swift

```swift
public init(
    name: String,
    organizationName: String? = nil,
    options: ProjectDescription.Project.Options = .options(),
    packages: [ProjectDescription.Package] = [],
    settings: ProjectDescription.Settings? = nil,
    targets: [ProjectDescription.Target] = [],
    schemes: [ProjectDescription.Scheme] = [],
    fileHeaderTemplate: ProjectDescription.FileHeaderTemplate? = nil,
    additionalFiles: [ProjectDescription.FileElement] = [],
    resourceSynthesizers: [ProjectDescription.ResourceSynthesizer] = .default
)
```

이러한 이니셜라이저를 가지고 있음

- name
    - 프로젝트 이름
- organizationName
    - organization의 이름
- options
    - tuist가 xcodeproj 파일을 만들 때의 옵션을 설정할 수 있음
- packages
    - SPM의 package 의미
- settings
    - 프로젝트 파일에 있는 build settings의 정보 설정
    - Dictionary로 값을 줄수 있음
        - https://xcodevuildsettins.com/
- targets
    - 프로젝트의 타겟 의미
- schemes
    - 프로젝트의 scheme 의미
- fileHeaderTemplate
    - 내장 Xcode 템플릿에 Custom으로 파일 헤더를 만들 수 있음

<br>

### Workspace.swift

workspace.swift는 Tuist에서 .xcworkspace 워크스페이스를 어떻게 만들지 정의하는 파일

```swift
public init(
    name: String,
    projects: [ProjectDescription.Path],
    schemes: [ProjectDescription.Scheme] = [],
    fileHeaderTemplate: ProjectDescription.FileHeaderTemplate? = nil,
    additionalFiles: [ProjectDescription.FileElement] = [],
    generationOptions: ProjectDescription.Workspace.GenerationOptions = .options()
)
```

이러한 이니셜라이저를 가짐

- name
    - 워크스페이스의 이름
- projects
    - Workspace에 등록할 프로젝트들의 경로
    - struct인 Path를 받지만 그냥 문자열 넣어도 무방
    - 기본 경로는 프로젝트의 루트 디렉토리를 기준으로 함
- schemes, fileHeaderTemplate, additionalFiels
    - Project.swift와 동일
- generationOptions
    - Tuist가 xcworkspace 파일을 만들 때의 옵션을 설정해줄 수 있음

<br>

### Config.swift

프로젝트 전역으로 쓰이는 설정을 할 수 있음

ex) Swift 버전이나 Xcode의 버전

Config.swift는 {프로젝트 루트 디렉토리}/Tuist/Config.swift에 있을 때만 적용됨

### Target

target은 사용할 모듈을 정의하는 struct

```swift
public init(
    name: String,
    platform: ProjectDescription.Platform,
    product: ProjectDescription.Product,
    productName: String? = nil,
    bundleId: String,
    deploymentTarget: ProjectDescription.DeploymentTarget? = nil,
    infoPlist: ProjectDescription.InfoPlist? = .default,
    sources: ProjectDescription.SourceFilesList? = nil,
    resources: ProjectDescription.ResourceFileElements? = nil,
    copyFiles: [ProjectDescription.CopyFilesAction]? = nil,
    headers: ProjectDescription.Headers? = nil,
    entitlements: ProjectDescription.Path? = nil,
    scripts: [ProjectDescription.TargetScript] = [],
    dependencies: [ProjectDescription.TargetDependency] = [],
    settings: ProjectDescription.Settings? = nil,
    coreDataModels: [ProjectDescription.CoreDataModel] = [],
    environment: [String : String] = [:],
    launchArguments: [ProjectDescription.LaunchArgument] = [],
    additionalFiles: [ProjectDescription.FileElement] = []
)
```

이러한 이니셜라이저를 가짐

- name
    - 타겟의 이름
- platform
    - iOs, macOS, tvOS, watchOS 같은 플랫폼
- product
    - app, appClips, staticFramesork, framework, unitTest 등
- productName
    - 만들어진 product의 이름
    - The vuild product name
- bundled
    - 프로젝트 파일을 열었을 때 보이는 Bundle Identifier
- deploymentTarget
    - 배포 타겟을 설정할 수 있음
- infoPlist
    - info.plist 정의
- sources
    - 소스코드의 경로 입력
    - 안에 문자열로 경로 입력해도 됨
- resources
    - 앞서서 resourceSyntheziers에서 Tuist가 Resources/의 리소스들을 자동으로 코드화한다 했는데 그때 이 리소스들이 어디에 있는지에 대한 경로 의미
    - 안에 문자열로 경로를 입력해도 됨
- copyFiles
    - 타겟에 대한 Build Phase 파일 복사 작업
- headers
    - 타겟에 대한 headers
- entitlements
    - 타겟에 대한 entitlements의 경로를 입력
- scripts
    - 타겟에 대한 build Phase 스크립트 작업
- dependencies
    - 타겟의 의존성에 대한 것을 의미
    - 라이브러리나 다른 모듈을 의존성으로 넣을 때 사용
- settings
    - 타겟의 세팅 정의
- coreDataModels
    - CoreData의 모델들의 경로랑 버전 정의
- environment
    - scheme에서 Edit Scheme… 버튼을 누르면 나오는 창에서 Environment Variables를 설정할 수 있는데 이때 enviroment를 설정하면 자동으로 생성
- launchArguments
    - scheme에서 Edit Scheme… 버튼을 누르면 나오는 창에서 Arguments Passed On Launch를 설정할 수 있는데 이때 launchArguments를 설정하면 자동으로 생성
- additionalFiles
    - 프로젝트를 생성할 때 자동으로 생겨나지 않는 파일들을 등록해놓으면 Xcode에서 볼 수 있음

<br>

> **프로젝트 생성**
> 

편집을 모두 마치고 아래 명령어를 입력하면 프로젝트가 생성됨

```swift
tuist generate
```

.xcodeproj, xcworkspace 파일 생성