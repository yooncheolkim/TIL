# 무식하게풀기
- 알고리즘 설계는 전략적선택을 해야한다.
- 문제의 특성을 이해, 동작 시간과 사용하는 공간 사이의 상충관계를 이해 하고, 적절한 자료구조 선택
- 컴퓨터 과학의 역사를 바꾼 중요한 알고리즘들을 설명한다.

## brute-force  무식하게 푼다.
- 완전탐색
- 컴퓨터를 이용하는 가장 가치있는 일


## 재귀 호출
- 우리가 보는 범위가 작아지면 작아질수록 각 조각들의 형태가 유사해지는 형태일때..
- 완전히 같은 코드를 반복해 실행하는 for 같은 반복문, 재귀호출이 좋은 예이다.
- 다양한 알고리즘을 수행하게 해주는 유용한 도구.

### 1부터 n까지의 합을 계산하는 반복함수
~~~c
int sum(int n){
    int ret = 0;
    for(int i = 1; i <= n ; i++){
        ret += i;
    }
    return ret;
}

int recursiveSum(int n){
    if(n ==1) return 1;
    return n+ recursiveSum(n-1);
}
~~~
- n개의 합을 구할때, 작업을 n개로 쪼개서, 하나하나 더하도록 조각이 되도록 한다.
- 한 조각을 처리하고, 나머지는 재귀호출이 해결하도록 한다.
- 더이상 쪼개지지않는 최소한의 조건에 도달했을떄, 답을 반환하는 조건문을 포함시켜야한다.
- 가장 작은 작업을 기저사례(base case) 라고 한다.
- 문제의 특성에 따라, 재귀 호출은 코딩을 훨씬 간편하게 해 줄 수 있는 강력한 무기가 된다.


#### 예제. 중첩 반복문 대체하기
- 0부터 차례대로 번호 매겨진 n개의 원소중 네개를 고르는 모든 경우를 출력하자
~~~java
for(int i = 0 ;i < n ; i++){
    for(int j = i+1; j < n; j++){
        for(int k = j+1; k< n ; k++){
            for(int l = k+1; l < n ; l++){
                System.out.print(i + " " + j + " " + k + " " + l);
            }
        }
    }
}
~~~

- 만약 5개를 골라야한다면? -> 5중 for문
- 골라야할 원소의 갯수가 달라진다면, for문을 사용할 수 없다.
- 작은 조각으로 나눠보자!
##### 조각 나누기
- 각 조각에서 하나의 원소를 고르고
- 이렇게 원소를 고른 뒤, 남은 원소들은 자기자신을 호출해 떠넘긴다.
- 이때 남은 원소들을 고르는 작업을 계속한다.
<br></br>
- 원소들의 총 개수
- 더 골라야할 원소들의 개수
- 지금까지 고른 원소들의 번호

~~~java
// n : 전체 원소수
// picked : 골라진 원소 번호
// toPick : 골라야할 원소 수
void pick(int n , List<int> picked, int toPick){
    //기저사례
    if(toPick == 0) {printPicked(picked); return;}

    //고를수 있는 가장 작은 번호 
    int smallest = picked.isEmpty() ? 0 : picked.last() + 1;

    // 원소 하나 고르기 smallest 부터 n까지
    for(int next = smallest; next < n ; next++){
        picked.add(next);
        pick(n,picked,toPick-1);
        picked.removeLast();
    }
}
~~~



### 예제. 보글 게임
- hasWord(y,x,word) = (x,y)에서 시작하는 단어 word가 존재한다.
- 모든 칸을 다 탐색한다.

~~~java
const int dx[8] = {-1,-1,-1,1,1,1,0,0};
const int dy[8] = {-1,0,1,-1,0,1,-1,1};


bool hasWord(int y, int x, string word){
    //기저사례 1 : 시작 위치가 범위 밖이면 무조건 실패
    if(!inRange(y,x)) return false;
    
    //기저사례 2 : 첫 글자가 일치하지 않으면 실패
    if(board[y][x] ! = word[0]) return false;

    //기저사례 3 : 단어 길이가 1이면 성공
    if(word.size() == 1) return true;

    for(int direction = 0 ; direction < 8 ; direction++){
        if(hasword(nextY, nextX, word.substr(1,word.length)))
            return true;
    }
    return false;
}
~~~
#### 시간복잡도
- 가능한 답 후보를 모두 만들기 때문에, 전부 세어보면된다.
- 최악의 경우 모든 경우를 다 해본다.
- 8<sup>n</sup>
- 그렇기 때문에 단어가 짧은 경우에만 해결할 수 있다.

### 완전탐색 레시피
1. 완전 탐색은 존재하는 모든 답을 하나씩 검사하므로, 걸리는 시간은 가능한답의 수에 정확히 비례한다. 최대 크기의 입력을 가정했을때 답의 개수를 계산하고, 시간안에 생성할 수 있는지 가늠한다. 만약 계산할 수 없다면, 3부의 다른장에서 소개하는 설계 패러다임을 적용해야한다.
2. 가능한 모든 답의 후보를 만드는 과정을 여러 개의 선택으로 나눈다. 각 선택은 답의 후보를 만드는 과정의 한 조각이 된다.
3. 그중 하나의 조각을 선택해 답의 일부를 만들고, 나머지 답을 재귀 호출을 통해 완성한다.
4. 조각이 하나밖에 남지 않은 경우, 혹은 하나도 남지 않은 경우에는 답을 생성했으므로, 이것을 기저 사례로 선택해 처리합니다.


### 이론적 배경 : 재귀 호출과 부분문제(p.154)
- problem, subproblem
- 동적계획법, 분할정복에서 사용되는 정의...

문제 : 주어진 자연수 수열을 정렬하라.<br></br>
문제 : {16,7,9,31}을 정렬하라.

- 재귀 호출을 논의할때, '문제'란 항상 수행해야 할 작업과 그 작업을 적용할 자료의 조합을 의미한다.
- 예를 들어, {1,2,3}을 정렬하는 문제와 {3,2,1} 을 정렬하는 문제는 서로 다르다.
- 보글문제에서 해당 위치에서 단어를 찾을수 잇냐는 문제는 -> 현재 위치에 첫글자가 있는지, 왼쪽위에서 나머지 글자를 찾을수있는지, 위에서 나머지 글자를 찾을수 있는지, 오른쪽 위에서 나머지 글자를 찾을 수 있는지...
- 첫글자가 있는지 확인하는것 이외의 문제들은 다 조각들이다. - 원래 문제의 부분 문제


### 문제. 소풍


~~~java
package Algo_Picnic;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {

    static class Pair{
        int o1 = 0;
        int o2 = 0;
        public Pair(int o1, int o2){
            this.o1 = o1;
            this.o2 = o2;
        }
    }

    static int C, n , m;
    static List<Pair> list;
    static int result = 0;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        C = scanner.nextInt();

        for(int  i = 0 ; i < C ; i ++){
            n = scanner.nextInt();
            m = scanner.nextInt();

            list = new ArrayList<>();
            for(int j = 0; j < m ; j++){
                int o1 = scanner.nextInt();
                int o2 = scanner.nextInt();
                list.add(new Pair(o1,o2));
            }
            list.sort((a,b) ->
                a.o1 < b.o1 ? -1 : 1
                );

            getSolve(n/2,new ArrayList<Integer>(),n/2);
            System.out.println(result);
            result = 0;
        }
    }

    //n : 짝의 갯수
    //picked : 골라진 짝의 인덱스 list
    //toPick : 골라야할 짝의 갯수
    static void getSolve(int n , List<Integer> picked, int toPick){

        //기저사례
        if(toPick == 0 && picked.size() >= n) {result++;}

        int min = picked.isEmpty() ? 0 : picked.get(picked.size()-1) +1;

        for(int i = min ; i < m ; i++){
            // i를 넣어도 되는지 검사
            if(isAddOk(picked, i)){
                picked.add(i);
                getSolve(n,picked,toPick-1);
                picked.remove(picked.size()-1);
            }
        }
        return;
    }

    static boolean isAddOk(List<Integer> picked, int i){
        Pair p = list.get(i);
        for(int idx = 0 ; idx < picked.size() ; idx++){
            if(list.get(picked.get(idx)).o1 == p.o1
            || list.get(picked.get(idx)).o1 == p.o2
            || list.get(picked.get(idx)).o2 == p.o1
            || list.get(picked.get(idx)).o2 == p.o2) return false;
        }
        return true;
    }
}

~~~


## 최적화 문제
- n개의 원소중 r개를 순서 없이 골라내는 방법의 수 : 최적화 문제가 아니다.
- n개의 사과중 r개를 골라 무게의 합을 최대화 하는 문제 : 최적화 문제!(최적의 답을 찾는 문제)
- 모든 최적화 문제는 완전탐색으로 해결 가능하다!

### 예제. 시계 맞추기
~~~java
package Algo_Clocksync;

import java.util.Scanner;

public class Main {

    static int linked[][] = {
            {1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0},
            {0,0,0,1,0,0,0,1,0,1,0,1,0,0,0,0},
            {0,0,0,0,1,0,0,0,0,0,1,0,0,0,1,1},
            {1,0,0,0,1,1,1,1,0,0,0,0,0,0,0,0},
            {0,0,0,0,0,0,1,1,1,0,1,0,1,0,0,0},
            {1,0,1,0,0,0,0,0,0,0,0,0,0,0,1,1},
            {0,0,0,1,0,0,0,0,0,0,0,0,0,0,1,1},
            {0,0,0,0,1,1,0,1,0,0,0,0,0,0,1,1},
            {0,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0},
            {0,0,0,1,1,1,0,0,0,1,0,0,0,1,0,0}
    };

    static int C;
    static int input[];

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        C = scanner.nextInt();

        for(int i = 0 ; i < C; i++){
            input = new int[16];
            for(int j = 0 ; j < 16 ; j++){
                input[j] = scanner.nextInt();
            }

            int result = getMinClick(input,0);
            System.out.println( result == 999999999 ? -1: result);
        }
    }
    // currInfo : 현재 상태
    // swtch : 지금 누를 스위치
    static int getMinClick(int[] currInfo, int swtch){

        if(swtch == 10) return isAlign(currInfo) ? 0 : 999999999;
        int ret = 999999999;
        for(int cnt = 0 ; cnt < 4 ; cnt++){
            ret = Math.min(ret, cnt+ getMinClick(currInfo, swtch+1));
            doClick(swtch, currInfo);
        }
        return ret;
    }

    //모두 12시가 되었는지 확인
    static boolean isAlign(int[] currInfo){
        for(int c : currInfo){
            if(c != 12) return false;
        }
        return true;
    }


    static void doClick(int switchNum, int[] currInfo){
        for(int i = 0 ; i < 16; i++){
            if(linked[switchNum][i] == 1){
                currInfo[i] = currInfo[i] + 3;
                if(currInfo[i] == 15) currInfo[i] = 3;
            }
        }
    }

    
}
~~~