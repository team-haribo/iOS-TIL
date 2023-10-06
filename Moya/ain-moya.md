# Moya

<br>

사용이 복잡하고 코드의 가독성이 좋지 않은 URLSession,

유지보수와 유닛테스트가 힘든 Alamofire 대신 등장하게된 **Moya !**

Moya는 Alamofire를 다시 추상화한 프레임워크로 재사용성을 높히고

개발자가 request, response에만 신경을 쓰도록 했다.

<br><br><br>

## TargetType

Request를 담당하는 MoyaProvider에 필요한 규격을 정의하는데 사용되는 프로토콜

<br>

**TargetType의 속성**

- baseURL: 서버의 도메인

- path: 도메인 뒤에 추가될 경로

- mathod: HTTP 메서드 ( GET, POST 등 )

- sampleData: 테스트용 Mock Data

- task: request에 사용될 파라미터

- validationType: 허용할 response의 타입

- headers: HTTP 헤더

<br>

```swift

enum Services {
    case test(authorization: String)
		case test2(authorization: String)
}

extension Services: TargetType {

    var baseURL: URL {
         return URL(string: "https://ainain.moyatest.com")!
     }
    
    var path: String {
        switch self {
        case .test:
            return "/"
        case .test2:
            return "/test"
        }
    }
    
    var method: Moya.Method {
        switch self {
        case .test:
            return .post
        case .test2:
            return .get
        }
    }
    
    var sampleData: Data {
        return "@@".data(using: .utf8)!
    }
    
    var task: Task {
        switch self {
        case .test, .test2:
            return .requestPlain
        }
    }
    
    var headers: [String : String]? {
        switch self {
        case .test(let authorization), .test2(let authorization):
            return["Content-Type" :"application/json","Authorization" : authorization]
        default:
            return["Content-Type" :"application/json"]
        }
    }
}
```

위의 예시 코드처럼 Switch문을 사용해서 여러 타입의 API에 대한 값들을 지정할 수 있다.

<br><br><br>

## 사용법

<br>

MoyaProvider 인스턴스를 생성하고 네트워크를 요청한다

```swift
let provider = MoyaProvider<Services>()

provider.rx.request(.test(authorization: authorization))
     .subscribe { [weak self] (event) in
         switch event {
         case .success(let response):
            print(response)
         case .error(let error):
             print(error.localizedDescription)
         }
     }
     .disposed(by: disposeBag)
```

request의 파라미터로 test를 지정하고 데이터를 받아와 바인딩하는 작업 !

<br><br><br>

## Moya의 장점

<br>

- 코드가 간결해지고 가독성이 높아진다.
- 여러 요청에서 동일한 코드를 재사용할 수 있다.
    
    서버의 기본 URL, 공통 헤더 및 기타 설정을 한 번 정의하면 모든 요청에서 이를 활용할 수 있다.
    
- TargetType 수정시 관련된 모든 API 요청에 자동으로 수정 내용이 반영되므로 유지 보수가 쉽다.