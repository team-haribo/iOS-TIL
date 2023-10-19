# Optional이란?
#### 값의 존재 여부를 나타내고 값이 있으면 해당 값을 저장 / 값이 없을 경우 "nil"을 저장시키는 Swift의 데이터 형식이다.
---

### Optional을 사용했을때 장점
- **안전성 향상:** Optional은 값의 존재 여부를 불확실한 상황에서 명시적으로 다룰 수 있게 합니다. 이로 인해 값이 없는 상황을 더 안전하게 처리할 수 있으며, 예상치 못한 nil 값으로 인한 오류를 방지할 수 있음

- **오류 처리:** 함수나 메서드가 실패할 수 있는 경우, Optional을 사용하여 오류를 나타낼 수 있습니다. 이를 통해 오류를 더 명시적으로 처리할 수 있고, 예외 상황을 처리하는 코드를 간소화할 수 있음

- **의미 전달:** 코드를 읽는 사람에게 값이 불확실한 상황임을 명확하게 전달할 수 있음 이로 인해 코드의 가독성을 향상시키고 의도가 명확해짐
<br>

### Optional을 사용했을때 단점
- **코드 복잡성 증가:** Optional을 다루는 코드를 작성하면 코드가 더 복잡해질 수 있움 특히 중첩된 Optional 값이 있는 경우 코드를 더 복잡하게 만들 수 있음

- **강제 언래핑 오류:** 값이 Optional로 래핑되어 있을 때 강제로 언래핑하지 않고 사용하면 런타임 에러가 발생할 수 있음, 따라서 올바른 처리가 없을 경우 오류가 발생할 수 있음

<br>

**값이 있는 Optional:** 값이 존재하는 경우를 나타내는 Optional. 이 경우 변수 또는 상수에 실제 값이 들어가며 값이 없는 경우 nil을 가짐

```swift 
var age: Int? // 값이 있을 수도 있고 없을 수도 있음
age = 30 // 값이 있는 Optional
age = nil // 값이 없는 Optional
```

<br>

**값이 없는 Optional:** 값이 없는 경우를 나타내는 Optional
이 경우 변수 또는 상수는 nil을 가짐

```swift
var name: String? // 값이 없을 수도 있음
name = nil // 값이 없는 Optional
```

#### Optional 바인딩 하는법
1. Optional 변수생성

```swift
var optional1: String?
```

2. 값이 있는지 확인 / 바인딩
```swift
if let unwrapped1 = optional1 {
    // 값이 있는 경우, unwrapped1에 값이 바인딩됩니다.
    print(unwrapped1)
} else {
    // 값이 없을떄
    print("값이 없습니다.")
}
```
3. 바인딩 (guard사용)
```swift
func process1() {
    guard let unwrapped1 = optional1 else {
        // 값이 없는 경우, 예외나 오류를 처리하고 함수종료함
        return
    }
    // 값이 존재하는 경우, unwrapped1에 값이 바인딩되어 함수 내에서 사용할 수 있음
    print(unwrapped1)
}
```
