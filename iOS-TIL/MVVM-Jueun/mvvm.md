## MVVM

#### MVVM이란?
: Model - View - ViewModel 3가지 그룹으로 이루어져 있는 패턴

<br>
<b>Model</b> <br>
다루게 될 데이터 즉, 데이터와 관련된 코드를 담고 있음(구조체, 네트워크 로직 JSON 파싱 코드) <br> View와 ViewMdel을 전혀 생각하지 않아도 됨 <br><br>
<b>Views</b> <br> 
앱의 UI에 대한 코드를 담고 있음 <br>
재사용성이 강조되며 중복된 코드를 줄이는 것이 중요함 <br><br> 

<b>View Models</b>
앱의 핵심적인 비즈니스 로직을 담고 있는 코드 계층
 MVC의 Controller와 비슷한 역할을 함(View의 요청에 따라 로직 실행, Model 변화에 따라 View를 refresh하는 등)

<br>

#### MVC vs MVVM
: MVC와 MVVM은 비슷하지만 가장 큰 차이점은 <b>View Controller의 책임이 줄어든 것</b>
MVC에서 Controller는
① Model의 데이터를 View에 맞게 뿌려주는 역할
② Business Logic을 수행하는 역할
MVVM에서의 Controller는
① 역할을 ViewModel에게 넘겨줌

<br>
#### MVVM의 장점
- ViewModel은 View로부터 독립적이며, View가 필요로 하는 데이터만 소유함
- View와의 의존성을 분리할 수 있음
- View Controller가 거대해지는 것을 방지할 수 있으며 유지보수, 재사용성, 테스트 등을 용이하게 만들어 줌

#### MVVM의 단점
- ViewModel과 View의 양방향 Binding 과정이 필요함
- 단순한 UI의 경우에는 MVVM이 너무 과할 수 있음
- 앱이 커질 수록 Data Binding 과정이 복잡해짐