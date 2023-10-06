## Moya

### Moya란?
: **Alamofire를 한 번 더 추상화**하여 네트워크 계층을 템플릿화하여 재사용성을 높이고 가독성을 높여 개발자는 오로지 **Response**와 **Request**에만 신경을 쓸 수 있도록 하여 생산성 네트워킹 라이브러리
+ Alamofire : URLSession을 추상화하여 보기 쉬운 형태로 네트워킹을 제공함

<br>

### 장점
- 컴파일시 API 엔드 포인트가 올바른지 체크
- Enum을 이용해서 언제, 어디에 사용될지 안전하게 (type-safe) 정의
- 유닛 테스트를 용이하게 만듦

<br>

### Moya 설치법
###### Cocoapods를 이용한 설치법
프로젝트 폴더에서 Pod init, Podfile에 아래와 같이 필요한 라이브러리 추가 → Pod install
```
pod 'Moya'
pod 'Moya/RxSwift'
pod 'Moya/ReactiveSwift'
pod 'Moya/Combine'
```
###### Swift Package Manager를 이용한 설치법
```
Xcode 에서 File → Add Packages
https://github.com/Moya/Moya.git 입력 후 설치
```

<br>

### API 작성
```
enum UserAPI {
    case LogIn(oAuthProvider : OAuthProvider, accessToken : String)
    case refreshToken
}
```
Enum을 사용해서 사용할 API 목록을 작성하면 보다 안전하게 사용할 수 있고, 유지보수의 관점볼 때 새로운 API를 추가할 때 편리함

<br>

### TargetType 작성
- **baseURL** : Server BaseURL 작성 [기본 도메인 작성]
- **path** : API path 지정 [기본 Path를 정의]
- **method** : HTTP method 지정 [어떤 방식으로 통신 할 것이냐]
- **sampleData** : Mock Data for Test
- **task** : Parameters for request 지정 [어떻게 데이터를 전송 할 것이냐]
- **validationType** : 허용한 response 정의 (기존 Alamofile의 .validate()처럼 response의 StatusCode에 따라 성공 유무 판단)
- **headers** : HTTP headers 적용 (기존의 인터셉터의 Adapter 역할을 할 수 있음 ex) 헤더에 JWT 값 넣기) [헤더에 어떤 값을 넣을 것이냐]

<br>

### Provider Wrapper 생성
기본적으로 Moya는 Provider 객체와 TargetType(서비스)만 있으면 네트워크 통신을 할 수 있음
#### -init
**endPointClosure** : 통신 요청을 지정해서 할 수 있음
**stubClosure** : 테스트를 할 때 사용
**plugins** : Custom 플러그인 원하는 방식으로 Request와 Response를 변경 할 수 있음
 #### requestSuccessRes
 => GET 호출
 => POST 호출
 => Parameter 호출
 #### requestDownload
 => Download 호출

<br>

### endpoint란?
: API가 서버에서 리소스에 접근할 수 있도록 가능하게 하는 url
메소드(GET 등)는 같은 url들에 대해서도 다른 요청을 하게끔 구별하게 해주는 항목이 바로 'endpoint’임