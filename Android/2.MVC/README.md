### 안드로이드에서 MVC 패턴
- [MVC-MVP-and-MVVM 종합안내서](https://academy.realm.io/kr/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/) 의 내용을 정리하였습니다.
#### 선행지식
- [unit test](https://www.slideshare.net/youngeunchoi12/springcamp2014-35347399)
- [의존성 주입](https://blog.perfectacle.com/2017/09/04/di-v1/)
#### 모델(Model)
- 데이터 + 상태 + 비즈니스 로직.
- View에 보여줄 데이터 나 상태를 유지 하거나, 상태를 업데이트 할수 있다.
- MVC,MVP,MVVM 에서 모델은 거의 같다고 한다..
#### 뷰(View)
- 사용자에게 보여지는 UI와 사용자에 의한 이벤트를 컨트롤러에게 알려준다.
- xml이외에 상태를 유지 하지 않기 때문에, 모델이나 controller에 종속적이지 않다.
#### 컨트롤러(Controller)
- 뷰에게 사용자 이벤트의 알림을 받으면
- 모델에 상태 변화를 알리고
- 이에 따라 뷰를 변경 시킨다.
- 모델과 뷰를 모두 유지 해야 한다.
- 안드로이드에서 Activity 나 Fragment가 이 역할을 한다..

<image src="https://user-images.githubusercontent.com/23315291/41496834-f5f5112e-7183-11e8-8fac-9b923a0f314b.PNG" height ="120" width="600">

#### 장점
- 모델과 뷰를 분리해준다.
- 모델의 테스트는 쉽다.
- 모델을 쉽게 테스트 할수 있다.
#### 단점(컨트롤러)
- 컨트롤러가 안드로이드 API에 깊게 종속 되므로 유닛 테스트가 어렵다.
- 컨트롤러가 뷰에 단단하게 결합되어 있다.
- 프로젝트가 커지면서, 컨트롤러가 해야 하는일이 많아진다.


#### 내생각...
- 그동안 아무 생각 없이 안드로이드 개발을 해왔는데 MVC패턴에서 가장 큰 차이는...
- 데이터를 유지하고 처리하는 **Model**인것 같다.
- 모델은 controller,view 어느곳에도 종속적이지 않기 때문에, 유닛 테스트도 쉽고, 용이성도 있다고 한다.

### reference
- [web에서 MVC패턴](https://opentutorials.org/course/697/3828)
- [ticTacToe source](https://github.com/ericmaxwell2003/ticTacToe)