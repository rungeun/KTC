# 알고리즘
참고 자료: 문제 해결을 위한 알고리즘 with 수학 (위키북스)
도서 정오표: https://wikibook.co.kr/algorithm-math/

## 목차

# Chapter 2
## 2.4 계산 횟수 예측하기
1초에 약 10^9번 계산 가능

전체 탐색: 일반적으로 나올 수 있는 모든 패턴을 확인하는 방법


### O 표기법

시간 복잡도: 병목이 되는 부분 찾기

#### 정수 시간

#### 선형 시간
FOR 1개 N

FOR 2개 N^2

FOR 3개 N^3

#### 지수 시간

2^N

### 이진 탐색과 로그

2^B -> 2^B-1 -> 2^B-3

B=log2(N)


# Chapter 3
## 3.1 소수 판정법
모든 합성수는 2이상, √2이하 정수로 나누어 진다

시간복잡도 O(√N)
```cpp
#include <cmath>
#include <cstdio>
#include <iostream>

bool isprime(int n);

int main(void) {
    int n;
    scanf("%d", &n);

    if (isprime(n))
        printf("prime");
    else
        printf("not prime");
    return 0;
}

bool isprime(int n) {
    int limit = pow(n, 0.5);  // 루트값 구하기
    for (int i = 2; i < limit + 1; i++) {
        if (n % i == 0) return false;
    }
    return true;
}
```

## 3.2 유클리드 호제법
시간 복잡도 O(log(A+B))

(1회 조작시 A+B의 값이 2/3 이하로 줄어들기 때문)

1. 큰 수를 작은 수로 나눈 나머지
2. 큰 수를 나머지로 변경
3. 한쪽이 0이 되면 종료, 0이 아닌쪽이 최대공약수


```cpp
#include <cstdio>
#include <iostream>

int main(void) {
    int a, b;
    scanf("%d %d", &a, &b);
    while (a >= 1 && b >= 1) {
        if (a < b) {
            b = b % a;
        } 
        else {
            a = a % b;
        }
    }
    a = a > b ? a : b;
    printf("%d", a);
}
```

3개 이상의 최대공약수

1. 1번째 수와 2번째 수의 최대공약수를 계산
2. 1번의 최대공약수와 3번째 수의 최대공약수를 계산
3. N-1번째의 최대공약수와 N번째 수의 최대공약수 계산 (=최대공약수)


```cpp
//문제 3.2.2 ID016
#include <cstdio>
#include <iostream>
#include <vector>
using namespace std;

int main(void) {
    int n, n2, j;
    vector<int> v;

    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &n2);
        v.push_back(n2);
    }
    for (j = 0; j < n-1; j++) {
        while (v[j] >= 1 && v[j + 1] >= 1) {
            if (v[j] < v[j + 1]) {
                v[j + 1] = v[j + 1] % v[j];
            } else {
                v[j] = v[j] % v[j + 1];
            }
        }
        v[j + 1] = v[j + 1] > v[j] ? v[j + 1] : v[j];  // k+1에 넣깉
    }

    printf("%d", v[j]);
}
```
FIXME: cstdio로 수정, vector로 수정
```cpp
//문제 3.2.3 ID017  
#include <iostream>
#include <algorithm>
using namespace std;

// 최대공약수를 리턴하는 함수
long long GCD(long long A, long long B) {
	while (A >= 1 && B >= 1) {
		if (A < B) B = B % A; // A < B라면, 큰 수를 b로 변경합니다.
		else A = A % B; // A >= B라면 큰 수를 a로 변경합니다.
	}
	if (A >= 1) return A;
	return B;
}

// 최소공배수를 리턴하는 함수
long long LCM(long long A, long long B) {
	return (A / GCD(A, B)) * B;
}

long long N;
long long A[100009];

int main() {
	// 입력
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];
	
	// 답 구하기
	long long R = LCM(A[1], A[2]);
	for (int i = 3; i <= N; i++) {
		R = LCM(R, A[i]);
	}
	
	// 출력
	cout << R << endl;
	return 0;
}
```


## 3.3 경우의 수와 알고리즘
- 곱의 법치

사건 1은 N가지, 사건 2는 M가지 => 사건1과 사건2가 일어나는 경우의 조합은 모두 `NM가지`

- 곱의 법칙 확장

사건 1 A가지, 2 B가지, 3 C가지 => `ABC가지`

M가지 선택지가 나오는 사건을 N번 조합은 `M^N가지`

- N개의 대상을 나열하는 방법

`n!` ex.3개의 정수 나열은 3!=6

- n개의 대상 중에서 r개를 나열하는 방법

`nPr` = n!/(n-r)!

- n개에서 r개를 선택하는 방법
  
`nCr` = n!/(r!(n-r)!)

FIXME: 동일
```cpp
//ID018
#include <iostream>
using namespace std;

long long N;
long long A[200009];
long long a = 0, b = 0, c = 0, d = 0; // 오버플로우를 피할 수 있게 long long 사용

int main() {
	// 입력
	cin >> N;
	for (int i = 1; i <= N; i++) cin >> A[i];
	
	// a, b, c, d의 개수 세기
	for (int i = 1; i <= N; i++) {
		if (A[i] == 100) a += 1;
		if (A[i] == 200) b += 1;
		if (A[i] == 300) c += 1;
		if (A[i] == 400) d += 1;
	}
	
	// 출력(답은 a * d + b * c)
	cout << a * d + b * c << endl;
	return 0;
}
```
//TODO: 116p~121p

## 3.4확률&기대값과 알고리즘
확률: 어떤 사건이 어느 정도로 일어나는지

기댓값: 1회 시행으로 얻어지는 평균적인 값

기댓값의 성형성: 합의 기댓값 = 기댓값의 합

(참고: ID023 ~ ID025는 전부 같은 유형임)
```cpp
//ID023 (기댓값 구하기)
#include <cstdio>
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int a, n, b = 0, r = 0;  // 입력, 개수, 블루합, 레드합
    double Answer;
    vector<int> blue;
    vector<int> red;

    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a);
        blue.push_back(a);
    }
    for (int i = 0; i < n; i++) {
        scanf("%d", &a);
        red.push_back(a);
    }
    for (int j = 0; j < n; j++) {
        b += blue[j];
        r += red[j];
    }
    Answer = b / n + r / n;
    printf("%.12lf\n", Answer);
    return 0;
}
```
```CPP
//ID024 (기댓값 구하기)
#include <cstdio>
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int p, q, n;  // 선택지 개수, 배점, 개수, 블루합, 레드합
    double Answer = 0;

    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d %d", &p, &q);
        Answer += q / p;
    }

    printf("%.12lf\n", Answer);
    return 0;
}
```

```CPP
//ID025 (기댓값 구하기)
#include <cstdio>
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int a, b, n;  // 
    double Answer = 0;
    int A=0, B=0;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a);
        A+=a;
    }
       for (int i = 0; i < n; i++) {
        scanf("%d", &b);
        B+=b;
    }
    Answer= A/3.0 + B*2.0/3.0;
    printf("%.12lf\n", Answer);
    return 0;
}
```
## 3.5 몬테카를로법: 통계적 접근 방법
: 난수를 사용한 알고리즘의 일종

n회의 랜덤 시행, m회 성공한 경우 확률은 m/n



```cpp
//code3-5-1 (파이값 구하기)
#include <iostream>
using namespace std;

int main() {
	int N = 100000000; // N은 시행 횟수(적절하게 변경해서 사용해 주세요)
	int M = 0;
	for (int i = 1; i <= N; i++) {
		double px = rand() / (double)RAND_MAX; // 0 이상 1 이하의 랜덤한 숫자 생성
		double py = rand() / (double)RAND_MAX; // 0 이상 1 이하의 랜덤한 숫자 생성
		// 원점에서의 거리 sqrt(px * px + py * py)가
		// 1 이하라면 원 안에 있는 것이므로, 조건을 "px * px + py * py <= 1"로 설정
		if (px * px + py * py <= 1.0) M += 1;
	}
	printf("%.12lf\n", 4.0 * M / N);
	return 0;
}
```
