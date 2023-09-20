# Closure

<br>

일정 기능을 하는 코드를 하나의 블럭으로 모아놓은 것

클로저는 Named Clousre & Unnamed Closure 둘다 포함하지만,

보통 Unnamed Closure, 즉 익명함수를 말한다!

<br><br><br>

## 클로저의 상태

<br>

- 전역 함수: 이름이 있으면서 어떠한 값도 획득하지 않는 상태

- 중첩 함수: 이름이 있으면서 다른 함수 내부의 값을 획득할 수 있는 상태

- 클로저 표현식: 이름이 없고 주변 문맥에 따라 값을 획득할 수 있는 축약 문법으로 작성한 형태

<br><br><br>

## 클로저의 특징

<br>

- 매개변수와 반환 타입을 생략할 수 있다.

- 변수 / 상수에 클로저를 대입할 수 있고 대입된 변수나 상수로 실행할 수 있다.

- 기존에 클로저를 대입한 변수나 상수를 새로운 변수나 상수에 대입할 수 있다.

- 대입과 동시에 클로저를 작성할 수 있다.

- 함수의 파라미터 타입으로 클로저를 전달할 수 있다.

- 함수의 반환 타입으로 클로저를 사용할 수 있다.

<br><br><br>

## 클로저 사용법

<br>

```swift
{ (Parameter) -> ReturnType in
    실행 코드
}
```

괄호를 이용하여 파라미터를 정의하고 → 를 이용해 반환타입을 명시한다.

`in`  키워드를 이용해 실행 코드와 분리한다.

<br><br>

```swift
//변수나 상수로 클로저 호출

let closure = { () -> String in
    return "Hi!"
}

closure()
```

```swift
//변수나 상수를 대입하지 않고 직접 실행

({ () -> () in
    print("Hi!")
})()
```

<br><br><br>

## 후행 클로저

<br>

함수나 메서드의 **마지막 인자**로 클로저를 넣었을 때

가독성이 떨어진다고 생각한다면 **후행 클로저** 기능을 사용하자 !

```swift
func testFunc(cl: () -> void) {

}

// 후행 클로저 사용 X
testFunc( cl: {

})

// 후행 클로저 사용 X
testFunc() {

}
```

클로저가 함수의 마지막 인자라면 마지막 매개변수 명을 생략한 후 함수 소괄호 외부에 클로저를 작성한다.

<br><br><br>

## 값 캡쳐

<br>

클로저가 생성될 때 해당 변수나 상수의 현재 상태를 저장하고,이를 클로저 내에서 사용할 수 있도록 하는 것

원본 값이 사라져도 클로저의 body안에서 그 값을 활용할 수 있다.

클로저는 타입에 관계 없이 **Reference Capture(참조)** 를 한다.

<br>

```swift
// 외부 범위에서 정수 변수를 선언
var outsideValue = 10

// 클로저 정의
let closure = {
    print(outsideValue) // 클로저 내부에서 outsideValue 사용
}

// 클로저 호출
closure() // 출력: 10

// 외부 범위에서 outsideValue의 값을 변경
outsideValue = 20

// 클로저 다시 호출 (클로저는 값을 캡쳐하므로 변경된 값을 사용)
closure() // 출력: 20
```

<br><br><br>

## 탈출 클로저

<br>

클로저가 비동기 작업에서 사용되거나 함수가 종료된 후에도 호출되는 경우

클로저가 함수의 범위를 탈출하여 함수의 실행이 완료된 후에도 호출될 수 있음을 의미한다.

<br>

함수를 선언할 때 매개 변수 유형 앞에 @escaping 키워드 명시해주기

```swift
func testFunc(completion: @escaping () -> Void) {
    print("Function start")
    
    DispatchQueue.global().async {
        completion()
    }
}

// 함수 호출과 함께 탈출 클로저 전달
testFunc {
    print("Completion closure called")
}

// 함수 호출 이후 코드
print("Function call completed")

// 출력 순서:
// Function start
// Function call completed
// Completion closure called
```
