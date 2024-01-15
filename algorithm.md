# 알고리즘
알고리즘 동계방학 수업 내용 정리 문서입니다.

## 목차
- [Chapter 1](#chapter-1)
  * [0.1 시간 복잡도(time complexity)](#01--------time-complexity-)
  * [0.2 c표준 입출력](#02-c------)
  * [0.3 컨테이너](#03-----)
      - [시퀀스 컨테이너 (Sequence Containers):](#----------sequence-containers--)
      - [연관 컨테이너 (Associative Containers):](#---------associative-containers--)
      - [해시 컨테이너 (Unordered Containers):](#---------unordered-containers--)
      - [컨테이너 어댑터 (Container Adapters):](#----------container-adapters--)
  * [1.4 std::vector](#14-std--vector)
    + [1.4.2 std::vector 가변 크기 배열](#142-std--vector---------)
      - [원소 삽입](#-----)
      - [원소 제거](#-----)
      - [그 외](#---)
    + [1.4.2 std::vector 할당자](#142-std--vector----)
  * [1.5 std::forward_list](#15-std--forward-list)
    + [1.5.1 std::forward_list 원소 사입과 삭제](#151-std--forward-list----------)
      - [원소 삽입](#------1)
      - [원소 삭제](#-----)
    + [1.5.2 std:forward_list의 기타 멤버 함수](#152-std-forward-list----------)
  * [1.6 반복자](#16----)
    + [시퀀스 컨테이너(추가 자료)](#---------------)
  * [1.7 std::list](#17-std--list)
    + [1.7.1 std::list 멤버 함수](#171-std--list------)
    + [1.7.3 양방향 반복자](#173--------)
    + [1.7.4 반복자 무효화](#174--------)
      - [연결 리스트](#------)
      - [벡터](#--)
  * [1.8 std::deque](#18-std--deque)
    + [1.8.1 덱의 구조](#181------)
  * [1.9 컨테이너 어댑터](#19---------)
    + [1.9.1 std::stack](#191-std--stack)
    + [1.9.2 std::queue](#192-std--queue)
    + [1.9.3 std::priority_queue](#193-std--priority-queue)
    + [1.9.4 어뎁터 반복자](#194--------)
  * [1.10 벤치마킹](#110-----)

# Chapter 1
## 0.1 시간 복잡도(time complexity)
정의: 입력값의 변화에 따라 연산을 실행할 때, 연산 횟수에 비해 시간이 얼마만큼 걸리는가

Big-O(빅-오) ⇒ 상한 점근

```
[ Big-O 표기법의 종류 ]
 O(1)   입력값이 증가하더라도 시간이 늘어나지 않는다
 O(n)   입력값이 증가함에 따라 시간 또한 같은 비율로 증가
 O(log n)  O(1)다음으로 빠른 시간 복잡도를 가진다
 O(n2)  입력값이 증가함에 따라 시간이 n의 제곱수의 비율로 증가
 O(2n)  Big-O 표기법 중 가장 느린 시간 복잡도를 갖는다(경우의 수가 2배씩 늘어남)
```
<img src="https://velog.velcdn.com/images/zion9948/post/d1cd2383-299c-4324-b026-88aceadac247/image.jpeg">


## 0.2 c표준 입출력

<p style="color:black; background:red" align="center"><b>! 내용 추가 !</b></p>

## 0.3 컨테이너
정의: 데이터를 담는 객체로, 여러 요소들을 저장하고 효율적으로 관리할 수 있게 도와주는 클래스 또는 템플릿입니다.

#### 시퀀스 컨테이너 (Sequence Containers):
1. 배열 (Array): 고정 크기의 배열.
2. 벡터 (Vector): 동적 배열, 크기 조절 가능.
3. 리스트 (List): 이중 연결 리스트.
4. 데크 (Deque): 양쪽 끝에서 삽입/삭제가 빠른 이중 엔드 큐.

#### 연관 컨테이너 (Associative Containers):
1. 세트 (Set): 중복을 허용하지 않는 키 집합.
2. 멀티세트 (Multiset): 중복을 허용하는 키 집합.
3. 맵 (Map): 키-값 쌍을 저장하며 중복된 키를 허용하지 않음.
4. 멀티맵 (Multimap): 중복된 키를 허용하는 키-값 쌍의 집합.

#### 해시 컨테이너 (Unordered Containers):
1. 해시셋 (Unordered Set): 해시 함수를 사용한 키 집합.
2. 멀티해시셋 (Unordered Multiset): 중복을 허용하는 해시 함수를 사용한 키 집합.
3. 해시맵 (Unordered Map): 해시 함수를 사용한 키-값 맵.
4. 멀티해시맵 (Unordered Multimap): 중복된 키를 허용하는 해시 함수를 사용한 키-값 쌍의 집합.

#### 컨테이너 어댑터 (Container Adapters):
1. 스택 (Stack): 후입선출(LIFO) 원칙에 따라 동작하는 어댑터.
2. 큐 (Queue): 선입선출(FIFO) 원칙에 따라 동작하는 어댑터.
3. 우선순위 큐 (Priority Queue): 우선순위에 따라 요소를 저장하고 접근하는 어댑터.

## 1.4 std::vector
c스타일 배열의 향상된 버전

- 가변 크기 배열
- 할당자

### 1.4.2 std::vector 가변 크기 배열
```cpp
#include<vector>
using namespace std;

// 크기가 0인 벡터 선언
vector<int> vec;

// 크기가 5, 초깃값을 지정한 벡터 선언
vector<int> vec = {1,2,3,4,5};

// 크기가 10이고, 모든 원소가 5로 초기화된 벡터 선언
vector<int> vec(10, 5);

vec[i]; // i 번째 요소에 접근
vec.at(i); // i 번째 요소에 접근
```

#### 원소 삽입

`push_back(값)` 벡터의 맨 마지막에 새로운 원소를 추가, O(1)

`insert(위치, 값)`  원하는 위치에 원소를 추가, O(n)   

ex) `vec.insert(vec.begin(), 0);` 맨 앞에 0 추가

---
아래 함수들을 사용하면 더 좋은 성능을 냄

`emplace_back(값)` 벡터의 맨 마지막에 새로운 원소를 추가

`emplace()` 원하는 위치에 원소를 추가

#### 원소 제거
`pop_back()` 맨 마지막 원소 제거, O(1)

1. `erase()` 오버로딩되어 있어서 2가지 방법으로 사용 가능, O(n)
2. `erase(위치)` 해당 위치의 원소 제거 

`erase(시작 위치, 끝 위치)` 시작 이상~끝 미만 원소 제거

ex) `vec.erase(vec.begin()+1, vec.begin()+4)`

`clear()` 모든 원소 제거

#### 그 외
`reserve(capacity)` 벡터에서 사용할 용량을 지정 (벡터의 크기를 변경하지는 않음)

벡터는 기본적으로 메모리에 필요한 만
큼의 용적을 할당하고, 필요하면 용적을 늘리거나 줄입니다. 이때 내부적으로 배열을 새로
만들고, 배열을 복사하고, 기존의 배열을 삭제하는 처리를 합니다. 이러한 작업이 너무 많이
반복되면 성능이 좋지 않아집니다. 그래서 필요한 만큼 한 번에 큰 용적을 명시적으로 추가
할 때 reserve 함수를 사용합니다.

`shrink_to_fit()` 여분의 메모리 공간을 해제

- 벡터의 용량이 벡터의 크기와 같게 설정
- 벡터의 크기가 더 이상 변경되지 않을 때 사용

### 1.4.2 std::vector 할당자

<p style="color:black; background:red" align="center"><b>! 템플릿 공부하고 보기 !</b></p>

## 1.5 std::forward_list

### 1.5.1 std::forward_list 원소 사입과 삭제
#### 원소 삽입
`push_front(값)` 맨 앞에 원소 삽입

`insert_after(위치, 값)` 특정 위치에 원소 삽입, O(1)

ex)
```cpp
using namespace std;

forward_list<int> fwd_list = {1, 2, 3};

fwd_list.push_front(0);         //맨 앞에 0 추가
auto it = fwd_list.begin();
fwd_list.insert_after(it, 5);   //맨 처음 원소 뒤에 5추가
fwd_list.insert_after(it, 6)    //같은 위치에 6추가
```

`emplace_front()` `emplace_after()` 사입 함수와 같은 기능, 추가적인 복사 또는 이동을 하지 않기 때문에 더 효율적

#### 원소 삭제
`pop_front()` 맨 앞 원소 제거

`erase_after()` 2가지 방법으로 사용가능

1. `erase_after(위치)` 해당 위치 원소 제거
2. `erase_after(fwd_list.begin(), fwd_list.end())` 맨 앞 원소 다음 ~ 맨 마지막 원소까지 원소 제거

### 1.5.2 std:forward_list의 기타 멤버 함수

<p style="color:black; background:red" align="center"><b>! 연산자 오버로딩, 클래스  공부하고 보기 !</b></p>

## 1.6 반복자
<b>정의:</b> [반복자(iterator)](https://tcpschool.com/cpp/cpp_iterator_intro)
란 STL 컨테이너에 저장된 요소를 반복적으로 순회하여, 각각의 요소에 대한 접근을 제공하는 객체입니다.

[즉](https://eehoeskrap.tistory.com/263), 포인터와 상당히 비슷하며, 컨테이너에 저장되어 있는 원소들을 참조할 때 사용한다.

반복자의 종류
 
 
- 입력 반복자(input iterator) :<br> **읽기**만 가능, **순방향** 이동, 현 위치의 원소를 **한 번만** 읽을 수 있는 반복자
 

- 출력 반복자(output iterator) :<br> **쓰기**만 가능, **순방향** 이동, 현 위치의 원소를 **한 번만** 쓸 수 있는 반복자 
 

- 순방향 반복자(forward iterator) :<br> **읽기/쓰기** 모두 가능, **순방향** 이동(++)이 가능한 **재할당**될 수 있는 반복자
 

- 양방향 반복자(bidirectional iterator) :<br> **읽기/쓰기** 모두 가능, **순/역 방향** 이동(--)이 가능한 반복자 
 

- 임의 접근 반복자(random access iterator) :<br>  **읽기/쓰기** 모두 가능, **임의 접근**, **양방향** 반복자 기능에 **+, -, += , -=, []** 연산이 가능

 반복자 종류의 계층
![반복자](https://github.com/rungeun/Algorithm/assets/132816679/c7cbfa02-c01c-466d-a6ff-f49036a7cef3)

기본 반복자의 ++와 +는 뒤로 이동, --와 -는 앞으로 이동

역 반복자의 ++와+는 앞으로 이동, --와 -는 뒤로 이동

### 시퀀스 컨테이너(추가 자료)
요소를 저장하고 요소를 찾는 순서를 제어할 수 있는 컬렉션(객체의 모음)

STL은 vector, deque, list라는 3가지 시퀀스 컨
테이너를 제공

- 벡터, 덱은 동적 배열
- 리스트는 이중 링크드 리스트

[이중 링크드 리스트](https://opentutorials.org/module/1335/8940)의 핵심은 노드와 노드가 서로 연결되어 있다는 점입니다 아래 그림을 보면 단순 연결 리스트(linked list)와는 다르게 노드가 이전 노드(previous)와 다음 노드(next)로 구성되어 있습니다(양방향으로 연결)

![이중링크드리스트](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/1335/2949.png)


## 1.7 std::list
- 이중 링크드 리스트
- forward_list에 비해 메모리를 더 사용
- 순/역 방향 이동 가능
- 템플릿 매개변수로 사용자 정의 할당자를 지정할 수 있음

### 1.7.1 std::list 멤버 함수
`emplace()`

`insert()`

`push_back()` O(1)

`emplace_back()`

`pop_back()` O(1)

`remove()`

`remove_if()`

`sort()`

`unique()`

`reverse()`

`size()` O(1)

### 1.7.3 양방향 반복자
어느 방향으로든 원하는 만큼 이동 가능(선형 시간 복잡도)

단, 임의접근 반복자 처럼 한 번에 이동하는 것은 불가능

### 1.7.4 반복자 무효화
컨테이너가 변경되어 특정 노드 또는 원소의 메모리 주소가 바뀌면 사용하던 반복자가 무효화될 수 있음

#### 연결 리스트
삽입, 삭제 동작에서  원소를 이동할 필요가 없으므로 반복자가 무효화되지 않는다.(반복자 유효)

#### 벡터
반복자 무효화가 될 수 있다
```cpp
vector<int> vec = {1, 2, 3, 4, 5};

auto v_it = vec.begin() + 4;    //v_it은 vec[4]원소를 가리킴

vec.insert(vec.begin() + 2, 0); //v_it 반복자는 무효화됨 
```
## 1.8 std::deque
덱(deque)은 양방햔 큐(double-ended queue)의 약자

### 1.8.1 덱의 구조
- `push_front()` `pop_front()` `push_back()` `pop_back()` 의시간 복잡도 O(1)
- 모든 원소에 대해 임의 접근 동작이 O(n)

- 덱 중간에서 원소 삽입, 삭제는 O(n) (실제로는 최대 O(n/2)로 동작)


자료 구조가 벡터와 비슷하다. 하지만, 앞/뒤쪽으로 모두 확장 가능

`push_front()`

`push_back()`

`insert()`

`emplace_front()`

`emplace_back()`

`emplace()`

`pop_front()`

`pop_back`

`erase()`

`shrink_to_fit`

ex)
```cpp
#include <iostream>
#include <deque>
using namespace std;

int main()
{
    deque<int> deq = {1, 2, 3, 4, 5};
    deq.push_front(0);  // 맨 앞에 0 추가
    deq.push_back(6);   // 맨 뒤에 6 추가

    deq.insert(deq.begin() + 2, 10);        //맨 앞에서 2칸 뒤에 10 추가
    
    deq.pop_back();     //맨 뒤 원소 삭제
    deq.pop_front();    //맨 앞 원소 삭제
    
    deq.erase(deq.begin() + 1);             //맨 앞에서 1번째 원소 삭제
    deq.erase(deq.begin() + 3, deq.end());  //맨 앞에서 3번째 ~ 맨 뒤 원소 삭제
    
    return 0;
}
```

## 1.9 컨테이너 어댑터
- `std::stack`
- `std::queue`
- `std::priority_queue`
  
### 1.9.1 std::stack
LIFO

한쪽 끝에서만 데이터 사입, 삭제 가능

모든 연산은 시간 복잡도가 O(1)

`push_back()`

`pop_back()`

`empty()`

`size()`

`top()`

`emplace()`

스택은 덱을 기본 컨테이너로 사용

`std::stack<int, std::vector<int>> stk;`  vector를 기본 컨테이너로 사용하는 스택

`std::stack<int, std::list<int>> stk;`  list를 기본 컨테이너로 사용하는 스택


### 1.9.2 std::queue
FIFO

`push()` 맨 뒤에 원소 추가

`pop()` 맨 앞에 원소 제거

덱을 기본 컨테이너로 사용한다.

모든 함수는 시간 복잡도가 O(1)

### 1.9.3 std::priority_queue
<p style="color:black; background:red" align="center"><b>!  !</b></p>

![우선순위큐](https://velog.velcdn.com/images/yun8565/post/a9f7850e-7839-45b8-aedc-4926772450ad/image.png)

### 1.9.4 어뎁터 반복자
반복자 지원 X

(언제든 특정 위치에 있는 원소를 참조할 수 있어서 모든 원소를 순회하는 작업을 할 필요가 없기 때문이다.)

## 1.10 벤치마킹
정의: 통계 데이터를 기반으로 더 나은 접근 방식을 결정하는 방법
[c++ 벤치마크 사이트](https://quick-bench.com/)







