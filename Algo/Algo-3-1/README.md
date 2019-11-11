# 반복문이 지배한다.

~~~c
    int majority1(const vector<int>& A){
        int N = A.size();
        int majority = -1, majorityCount= 0;
        for(int i = 0 ; i < N; i++){
            //A에 등장하는 A[i]의 수를 센다.
            int V = A[i], count = 0;
            for(int j = 0 ; j < N ; j++){
                if(A[j] == V){
                    count++;
                }
            }

            if(count > majorityCount){
                majorityCount = count;
                majority = V;
            }
        }
        return majority;
    }
~~~

위 코드의 수행시간은 N<sup>2</sup>

~~~c
    int majority2(const vector<int>& A){
        int N = A.size();
        vector<int> count(101,0);
        for(int i = 0 ; i < N ; i++){
            count[A[i]]++;
        }

        int majority = 0;
        for(int i = 1; i <= 100 ; i++){
            if(count[i] > count[majority]){
                majority = i;
            }
        }
        return majority;
    }
~~~

위 코드의 수행시간은 N+100 -> 즉 N


## 선형 시간 알고리즘

N개의 수치가 주어 졌을때 지난 M개의 이동평균? 측정치를 계산하는 방법은?
~~~c
vector<double> movingAverage1(const vector<double>& A, int M){
    vetor<double> ret;
    int N = A.size();

    for(int i = M-1 ; i <N ; i++){
        double partialSum = 0;
        for(int j = 0 ; j <M; j++){
            partialSum+= A[i-j];
            ret.pusu_back(partialSum / M);
        }
    }
    return ret;
}
~~~
위 코드의 수행시간은 M*(N-M+1) = NM - M<sup>2</sup> + M

969세의 할아버지가 10만일의 측정을 한다면, 253억회.

더 간단하게 구하는 방법은?

~~~javascript
    List<double> movingAverage2(List<double> A, int M){
        ArrayList<double> ret = new ArrayList<>();
        int N = A.size();
        double partialSum = 0;
        for(int i = 0 ; i < M-1; i++)
            partialSum += A[i];
        for(int i = M-1; i < N; i++){
            partialSum += A[i];
            ret.add(partialSum/M);
            partialSum -= A[i-M+1];
        }
        return ret;
    }
~~~

하나 더하고, 하나빼고 하면서, 끝까지 한번으로 끝내기

시간 : M + N-M = N

*코드수행 시간은 입력 N에 정비례한다. -> 선형 시간*


## 선형 이하 시간 알고리즘
대표적으로 정렬된 수에서 이진 탐색 -> logN

- binsearch(A[],x) = 오름차순으로 정렬된 배열에서 찾고 싶은 값 x가 주어질때 A[i-1] < x <= A[i] 인 i를 반환


## 지수 시간 알고리즘
다항시간 : N<sup>2</sup> , N<sup>100</sup>, N<sup>5</sup> ....
다항시간 보다 오래 걸리는 알고리즘


**만들 줄 아는 음식의 목록과, 해당 음식을 못 먹는 친구들의 목록이 주어질때, 친구가 먹을 수 있는 음식이 최소한 하나씩 있으려면 최소 몇가지 음식을 해야 할까요?**

### 모든 답 후보를 평가하기

첫번재 요리를 만들지 말지 결정하고, 두번째 음식 만들지 말지 결정하고... 계속반복

모든 답 세어보기 -> 재귀 호출 이용하여 푼다...

~~~javascript
const static int INF = 987654321;
//이 메뉴로 모두가 식사 가능한지 판단
bool canEverybodyEat(List<int> menu)
//요리할 수 있는 음식의 종류 수
int M;
//food번째 음식을 만들지 말지 결정
int selectMenu(List<int> menu, int food){
    if(food = M){
        if(canEverybodyEat(menu)) return menu.size();
        return INF;
    }
    // 이 음식을 만들지 않을 경우 
    int ret = selectMenu(menu, food+1);

    //이 음식을 만드는 경우
    menu.add(food);
    ret = min(ret, selectMenu(menu,food+1));
    menu.remove(menu.size()-1);
    return ret;
}
~~~

- 모든 답을 다 확인하기 때문에, 만들 수 있는 답의 수에 비례한다.
- 만들어지는 답의 경우는 2<sup>M</sup>
- 여기에 canEverybodyEat() 를 수행한다.
- 즉 N*M*2<sup>M</sup>
- 요리가 10가지 내외라면, 금방 끝나지만... N이 증가할때마다. 엄청나게 증가한다...


**소인수 분해의 수행시간**
- 입력으로 주어지는 갯수에 따라 증가하는게 아니라, 크기에 따라 달라지는 문제


~~~c
vector<int> factor(int n ){
    if(n ==1) return vector<int>(1,1);
    vector<int> ret;
    for(int div = 2; n >1; div++){
        while(n % div == 0){
            n /= div;
            ret.push_back(div);
        }
    }
    return ret;
}
~~~
소수일 경우, N-1 번 실행된다.



### 시간 복잡도
시간 복잡도 : 알고리즘이 실행되는 동안 수행하는 기본적인 연산의 수를 입력의 크기에 대한 함수로 표현

- 입력에 대한 수행시간이 달라지는 경우를 고려, 최선/최악의 경우를 따져야한다.

**시간복잡도 분석 연습**

선택 정렬과 삽입 정렬
~~~c
void selectionSort(vector<int> A){
    for(int i = 0 ; i < A.size(); i++){
        int minindex = i;
        for(int j = i+1; j <A.size(); j++){
            if(A[minIndex] > A[j])
                minIndex = j;
        }
        swap(A[i], A[minIndex]);
    }
}

void insertionSort(vector<int> A){
    for(int i = 0 ; i < A.size(); i++){
        // A[0...i-1]에서 i를 끼워 넣는다.
        int j = i;
        while(j >0 && A[j-1] > A[j]){
            swap(A[j-1], A[j]);
            j--;
        }
    }
}
~~~
선택정렬 : N<sup>2</sup>

삽입 정렬 : 최선 N, 최악 N<sup>2</sup>

-> 그러므로, 임의의 배열이 주어지면, 삽입정렬이 더 빠르다.


### 수행시간 어림 짐작하기
**입력의 크기를 시간 복잡도에 대입해서 얻은 반복문 수행 횟수에 대해, 1초당 반복문 수행 횟수가 1억을 넘어가면 시간 제한을 초과할 가능성이 있다.**

1차원 배열에서 연속된 부분 구간 중 그 합이 최대인 구간을 찾는 문제(구간 합 문제)
~~~javascript
const int MIN = Integer.MIN;
int inefficientMaxSum(List<int> A){
    int N = A.size(), ret = MIN;
    for(int i = 0 ; i < N ; i++){
        for(int j = i; j < N; j++){
            int sum = 0;
            for(int k = i ; k <= j ; k++)
                sum += A[k];
            ret = max(ret,sum);
        }
    }
    return ret;
}
~~~
전체 다 하면, N<sup>3</sup>

~~~c
int betterMaxSum(const vector<int> A){
    int N = A.size(); ret = MIN;
    for(int i = 0; i < N ; i++){
        int sum = 0;
        for(int j = i ; j < N; j++){
            //구간 A[i..j]의 합을 구한다.
            sum+= A[j];
            ret = max(ret,sum);
        }
    }
}
~~~
이렇게 하면, N<sup>2</sup>

이 두가지 방법 말고, 분할 정복을 이용한 nlogn, 동적계획법을 이용한 N 을 만들수 있다.