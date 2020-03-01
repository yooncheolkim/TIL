# 동적계획법
- cache : 이미 계산한 값을 저장해 두는 메모리의 장소
- 중복되는 부분 문제 : 두 번 이상 계산되는 부분 문제

### 예제. 재귀 호출 이항계수
~~~c
int bino(int n , int r){
    //기저사례 : n=r(모든 원소를 다 고르는 경우) 혹은 r=0 (고를 원소가 없는 경우)
    if(r == 0 || n == r) return 1;
    return bino(n-1, r-1) + bino(n-1, r);
}
~~~
- 중복되는 부분이 굉장히 많다.
- bino(25,12) 를 계산하기 위해서는 1천만 번의 함수 호출이 필요하다...

### 예제. 메모이제이션을 이용한 이항계수
~~~c
//-1로 초기화해 둔다.
int cache[30][30];
int bino2(int n, int r){
    //기저사례
    if(r == 0 || n == r) return 1;
    //-1이 아니라면 한 번 계산했던 값이니 곧장 반환
    if(cache[n][r] != -1)
        return cache[n][r];
    //직접 계산한 뒤 배열에 저장
    return cache[n][r] = bino2(n-2,r-1) + bino2(n-1,r);
}
~~~
- 모든 문제가 한번씩만 계산된다는 것을 보장할 수 있다.

### 메모이제이션을 적용할 수 있는 경우
- 입력이 같을때, 같은 값만을 리턴하는 경우. (참조적 투명성?)

### 구현 패턴
- 항상 기저사례를 먼저 처리한다. 입력이 벗어난 경우 등을 기저사례로 처리한다.
- 함수 반환 값이 항상 0이상이면, cache[]를 모두 -1로 초기화한다.
- ret가 항상 cache[a][b]의 참조형으로 한다. 이렇게 하지 않으면 헷갈림
- 맨 처음에 초기화 하는게 굉장히 중요하다. c에서는 memset, java 는 Arrays.fill

~~~c
//사용 예
int cache[2500][2500];
//반환 값은 항상 int형 안에 들어가는 음이 아닌 정수.
int someObscureFunction(int a, int b){
    //기저사례 처리
    if(...) return ...;
    //(a,b) 에 대한 답을 구한적이 있으면 곧장 반환
    int& ret = cache[a][b];
    if(ret != -1) return ret;
    //여기에서 답을 계산한다.
    ///
    return ret;
}
int main(){
    memset(cache, -1, sizeof(cache));
}
~~~

### 메모이제이션의 시간 복잡도 분석
- 주먹구구식으로 계산하기
- (존재하는 부분 문제의 수) * (한 부분 문제를 풀 때 필요한 반복문의 수행 횟수)


### 예제. 외발 뛰기
1. 완전 탐색으로 가능한가?
    - 완전 탐색으로 가능하다!
    - 하지만 도달할 수 없는 경우, n = 100이면, 제한 시간을 초과해 버린다.
    - 중복되는 부분 문제들이 굉장히 많다.
2. 메모이제이션 이용하기
~~~c
int jump2(int y, int x){
    if(y >= n || x >= n ) return 0;
    if(y == n-1 && x == n-1) return 1;
    //메모이제이션
    int& ret = cache[y][x];
    if(ret != =1) return ret;
    int jumpSize = board[y][x];
    return ret = jump(y+jumpSize, x) || jump(y,x+jumpSize);
}
~~~
#### 동적 계획법 레시피
- 주어진 문제를 완전 탐색을 이용해 해결합니다.
- 중복된 부분 문제를 한 번만 계산하도록 메모이제이션을 적용합니다.




### 문제. 와일드 카드
