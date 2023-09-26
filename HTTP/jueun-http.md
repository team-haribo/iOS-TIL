## HTTP 통신

### HTTP 통신이란?
: URL 기반으로 클라이언트에서 요청을 보내고, 서버로부터 응답을 받는 형태의 통신

<br>

### URLSession
- HTTP / HTTPS 기반의 URL로부터 데이터를 다운로드하거나 업로드하는 API를 제공하는 클래스
- URLSession은 자체적으로 **비동기적으로 작동**하게 구현되어있으므로 따로 비동기 처리할 필요가 없음
- 대신 completionHandler를 작성할 때 UI 관련 작업을 수행한다면 반드시 Main 스레드에서 작업해주어야함

<br>

### URLSession 종류
URLSession의 종류는 Configuraion 객체에 의해 결정
- 공유 세션 : **URLSession.shared()**
  - 기본 요청을 위한 세션
  - configuration 객체 없음
  - 사용자 정의 불가

- 기본 세션 : **URLSession(configuration: .default)**
  - 디스크에 기록함 (캐시, 쿠키, 자격 증명)
  - delegate 지정 가능 (순차적으로 데이터 처리)

- 임시 세션 : **URLSession(configuration: .ephemeral)**
  - 디스크에 데이터를 쓰지 않음(캐시, 쿠키, 인증 등)
  - 메모리에 올려서 세션을 연결하고, 세션 만료 시 데이터 사라짐 (비공개 세션이라 생각하면 됨)

- 백그라운드 세션 : **URLSession(configuration: .background)**
  - 백그라운드에서 업로드, 다운로드가 가능함
  - 별도의 프로세스가 모든 데이터 전송을 처리 (앱이 중지되거나 종료되어도 계속함)

<br>

 ### URLSessionConfiguration
 - URLSession은 configuration이라는 객체를 가지고 있음
- 업로드할 지 다운로드할 지 등의 행동과 규칙을 정의하는 객체
- URLSession 객체를 초기화 하기 전에 가장 먼저 작업해야할 첫 단계이며 타임아웃 값, 캐싱 정책, HTTP 헤더와 같은 값들로 구성되어 있음
- **종류는 .default, .ephemeral, .background 총 3종류임**
- 여기서 주의해야 할 점은 configuration의 복사본으로 session을 셋팅하게 되어 session이 생성되고 난 이후에 configuration 객체가 변동되어도 session은 변하지 않음
- **configuration 규칙을 수정하고 싶으면, 새로운 configuration으로 새로운 session을 만들어야 함**

<br>

### Session 생성
```let sharedSession = URLSession.shared()```
```let defaultSession = URLSession(configuration: .defualt)```
```let ephemeralSession = URLSession(configuration: .ephemeral)```
```let backgroundSession = URLSession(configuration: .background)```

<br>

### URLSessionTask
- 각 세션 내에서는 작업(Task)을 추가할 수 있음
- 각 작업은 특정 URL에 대한 요청을 의미하며, HTTP 리디렉션이 될 수도 있음
- url 주소만으로 요청할때는 URL 객체를 이용하고ㅜ주소와 HTTP 메소드, Body까지 설정해야 할 때는 URLRequest 객체를 이용하면 됨
    **URL 객체**
    string값으로 되어있는 주소를 URL 객체로 생성
    
    **URLRequest 객체**
    URLRequest는 캐싱 정책, HTTP 메소드, HTTP Body 등을 설정할 수 있음

<br>

### URLSessionTask 종류
- URLSessionDataTask (HTTP GET)
  - 응답 데이터를 받아서 Data 형태의 객체를 받아오는 작업
  - JSON, XML, HTML
- URLSessionUploadTask (HTTP POST/PUT)
  - Data 객체 또는 파일 형태의 데이터를 서버로 업로드하는 작업 (백그라운드 O)
- URLSessionDownloadTask
  - 파일 형태의 데이터를 다운로드하는 작업 (백그라운드 O)
  - 일시정지, 재개, 취소 가능
- 웹소켓 작업은 URLSession이 아니라 WebSocket 프로토콜을 사용

<br>

### Task 상태 제어
- ancel() : 작업을 취소한다.
- resume(): 일시중단된 작업을 다시 시작한다.
- suspend() : 작업을 일시중단한다.
- state (get-only) : 작업의 상태
- priority (get-only) : 작업의 우선순위 (0.0 ~ 1.0)

Task 객체를 만들고 난 뒤 함수를 실행해도 되고 Task 객체를 만드는 동시에 바로 실행해도 됨