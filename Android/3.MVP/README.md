### 안드로이드에서 MVP 패턴
- [MVC-MVP-and-MVVM 종합안내서](https://academy.realm.io/kr/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/) 의 내용을 정리하였습니다.
- [MVC 먼저 선행학습 하기..](https://github.com/yooncheolkim/TIL/tree/master/Android/2.MVC)
#### 모델(Model)
- MVC와 동일

#### 뷰(View)
- MVC에서 컨트롤 역할을 했던 Activity, Fragment는 뷰이 일부가 된다.
- 그렇기 때문에, MVC에서 Acitivity가 View를 동작 시켰던 자연스러운 현상을 그대로 두어도 상관없다.
- View의 동작(ex.setbutton settextview, reset 등등)을 구현하도록 하는 interface를 만들어 Presenter는 이 interface만 가지도록 하는것이 좋다.
- **이렇게 하면 특정 뷰와 결합되지 않고 가상 뷰를 구현해 간단한 유닛 테스트를 실행할수 있다!**

#### 프리젠터(Presenter)
- **인터페이스이다.**
- 프리젠터는 절대로 어떤 안드로이드 API의 코드를 참조해서는 안된다.
- View(Activity or Fragment) 에게 **Ask View to setup itself!**
- Controller에서 발생하는 테스트, 모듈화 등의 문제가 해결될수 있다.
<image src="https://user-images.githubusercontent.com/23315291/41497166-f96be36c-718a-11e8-89bf-6784a3d70525.PNG" height="120" width="600"> 

#### 내생각...
- MVC와