### Single 클래스
- Observable 의 특수한 형태
- Observable은 데이터를 무한으로 발행할수 있지만, Single은 오직 1개의 데이터만을 발행합니다.
- 보통 결과가 유일한 서버 API를 호출할 때 유용하게 사용합니다.
<image src= "https://user-images.githubusercontent.com/23315291/45141428-d21a3380-b1f0-11e8-9974-295d62635490.png" >
- 데이터 하나가 발행과 동시에 종료된다. 

#### Observable에서 Single 클래스 사용
- 