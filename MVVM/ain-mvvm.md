# MVVM

MVC의 View와 Controller 사이의 의존성이 높다는 문제를 해결하기 위해 등장한 패턴 ! <br> <br> <br> <br> 




### MVVM의 구성 요소

---

<br> <br>
**Model**

- 앱의 데이터를 나타내고 비즈니스 로직을 포함하는 부분
- 데이터를 가져오고 가공하는 역할
    
    → MVC의 Model과 유사
    

<br> 

**View**

- UI를 관리하는 부분으로 앱의 화면을 구성한다.
    
- ViewModel로부터 데이터를 가져와서 표현하며, 사용자와의 상호 작용을 ViewModel에 부탁한다.
    
    → MVC의 View와 유사하지만 MVC, MVP와는 다르게 
    
        MVVM의 View는 보이는 부분에 대한 설정을 스스로 직접 한다.
    

<br>

**ViewModel**

- Model과 View 사이의 **매개체** 역할
- View로부터 전달받는 요청을 처리하는 로직을 담고 있으며, Model에 변화가 생기면 View에 알린다.
- Presentation Logic을 처리하고, 중간 다리 역할을 한다.
    
    → MVC의 Controller와 유사한 역할을 하며, 데이터와 화면 표시에 관련된 로직을 처리한다.
    

<br> <br>

### MVVM의 동작 흐름

---

<br>

1. View에 들어온 Event를 View Model에게 알림
   
2. View Model은 Model을 업데이트
   
3. Model이 변화하면 View Model에 알려지고, View Model과 바인딩되어있는 View가 업데이트 됨

<br> <br>


### MVVM의 특징

---

<br>

- MVVM은 MVC와 달리 ViewController를 View로 취급한다.

- View는 ViewModel을 참조한다.
  
- View는 Model을 참조하지 않는다.
  
- ViewModel은 Model을 참조한다.
  
- View와 ViewModel 사이에 데이터 바인딩을 설정하여 View가 스스로 데이터를 가져와 표시하므로 의존성이 낮다.
  
- Presentation Logic(화면 표시와 관련된 로직)은 ViewModel에서 처리하여 코드의 재사용성을 높인다.


<br> <br>

각 구성 요소가 역할을 명확하게 분리하여 코드의 재사용성과 유지 보수성을 높이는데 도움을 준다. 

View와 ViewModel 사이의 **데이터 바인딩**을 통해 View가 스스로 데이터를 표현하고 갱신하므로

View와 ViewModel 사이의 의존성을 최소화한다. ( 느슨한 결합 ! ) 

View와 ViewModel은 느슨하게 결합되어 있기 때문에 두 요소를 각각 독립적으로 테스트할 수 있다.


<br>

```
💡 MVVM의 데이터 바인딩?

MVVM에서 View와 ViewModel 사이의 상호작용을 가능하게 하는 메커니즘
이를 통해 View와 ViewModel이 서로 독립적으로 작동하면서도 데이터의 양방향 흐름을 제공하는 것!
```


