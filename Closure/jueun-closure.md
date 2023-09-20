## 클로저

<br>
### 클로저란?
: func 키워드를 사용해 이름을 붙여주는 함수를 뜻함 
사용자의 코드 안에서 전달되어 사용할 수 있는 로직을 가진 중괄호로 구분된 코드의 블럭

<br>
### Named Closure
: 우리가 평소 선언해왔던 이름이 있는 함수. 하지만
이를 클로저라 부르지 않고, 그냥 함수라고 부르는 것 뿐
(그치만 클로저란 사실은 변함 없음)
```Swift
func Hi() {
    print("Hi Closure")
}
```

<br>
### Unnamed Closure
: 이름을 붙이지 않고 사용하는 함수, 익명함수라고도 함
클로저하면 보통 unnamed closure를 말함
```Swift
let closure = { print("Hi Closure") }
```

<br>

### 클로저를 쓰는 이유
: 한 두 번 쓰고 말 것인 함수로 구현하여 메모리를
낭비할 필요가 없기 때문에 클로저를 사용함

<br>

### 클로저는 1급 객체이다?
클로저는 1급 객체로, 변수 형태로 저장할 수 있고 함수의 파라미터로도 넘길 수 있음

여기서 말하는 1급 객체란 함수형 프로그래밍에서 쓰이는 개념으로, 아래의 조건을 만족하는 객체를 말함

1. 변수나 상수에 저장 및 할당이 가능
2. 함수의 파라미터로 전달이 가능
3. 함수에서 리턴이 가능

<br>

### 클로저 표현식
```Swift
{(Parameters) -> Return Type in
    실행 구문
}
```
클로저 헤드와 클로저 바디로 나누어져 있는데 이 둘을 구분하는 것이 <b>in</b>이라는 키워드임

<br>

### 후행 클로저(Trailing Closure)
: 마지막 매개변수 이름을 생략한 후 함수 소괄호 외부에 클로저를 구현할 수 있게 해주는 것
```Swift
//후행 클로저 미사용
let onAction = UIAlertAction(title: "On", style: UIAlertAction.Style.default, handler: {
    //실행 코드
})

//후행 클로저 사용
let onAction = UIAlertAction(title: "On", style: UIAlertAction.Style.default) {
    //실행 코드
})
```

<br>

### 클로저의 축약 표현
- 타입 생략
- return 생략
- 매개변수 생략
- 연산자만 표기