# Auto Layout이란?
#### AutoLayout은 사용자 인터페이스의 구성 요소들을 동적으로 정렬 및 크기 조절하는 방법을 제공하는 레이아웃 시스템입니다. 
---

### VFL [Visual Format Language] 란?
#### AutoLayout에서 제약 조건을 설정하기 위한 간결하고 시각적인 언어입니다. VFL을 사용하면 복잡한 레이아웃을 표현하고 관리하는데 도움이 된다.

#### VFL에 주요 특징 2가지
- 1번: VFL은 문자열로 작성되며, 제약 조건을 명확하게 정의하는 데 사용됩니다.
- 2번: VFL은 사람이 읽기 쉽고 이해하기 쉬운 형태로 제약 조건을 표현함으로 복잡한 레이아웃을 더 직관적으로 다룰 수 있습니다.

---

### Priority [우선순위]
- AutoLayout에서 각 제약 조건은 우선순위를 가지며, 우선순위를 조절하여 다양한 레이아웃 시나리오를 처리할 수 있습니다.
---
### Intrinsic Content Size
- 각 뷰는 자체적으로 내재된 콘텐츠 크기를 가지며, AutoLayout은 이 내재된 크기를 기반으로 레이아웃을 조절합니다.
---
###  간접 및 직접 제약 조건
- AutoLayout은 간접적인 방식으로 설정된 제약 조건을 활용하여 직접적인 크기 및 위치 제약 조건을 생성합니다.

---
#### NSLayoutConstraint를 사용하여 redView를 view에 대한 Auto Layout 제약 조건을 설정하는

```swift
NSLayoutConstraint.activate([
            redView.topAnchor.constraint(equalTo: view.topAnchor, constant: 20),
            redView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20),
            redView.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -20),
            redView.bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: -20)```