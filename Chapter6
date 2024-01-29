# Chapter 6
## 6.1 들어가며
### 구성
정점(vertex)의 집합과 정점들을 서로 잇는 에지(edge)의 집합으로 구성됨

그래프는 객체(정점)와 객체 사이의 관계(에지)를 표현할 수 있어서 유용

`노드와 정점은 같은 의미`

G = <V, E>  (V: 정점의 집합, E:에지의 집합)

- __방향 그래프__: 에지가 특정 정점에서 다른 정점으로 향하는 방향이 있음
- __무방향 그래프__: 특정 방향을 가리키지 않음

<br>

- __가중 그래프__: 에지에 가중치(cost)가 있음
- __비가중 그래프__: 가중치가 없음

<br>

- __셀프 에지__: 에지가 자기 자신을 연결

<br>

- __하이퍼 그래프__: 여러 정점들을 동시에 연결하는 에지를 가질 수 있음
- __혼합 그래프__: 하나의  그래프 안에 방향 에지와 무방향에지를 함께 가질 수 있음

![graph6-1](https://github.com/rungeun/KTC/assets/132816679/fe16c217-bb6f-4b2b-be6f-0655de8cfaa8)

## 6.2 그래프 순회 문제

### 6.2.1 너비 우선 탐색 BFS    
>시작 정점에서 시작하여 점차 탐색 범위를 넓혀 나가는 방식
#### 과정
1. 시작 정점을 경계에 추가(경계는 이전에 방문했던 정점들)
![graph6-3](https://github.com/rungeun/KTC/assets/132816679/ae42837b-3a54-4252-9b60-592ead6fcee7)
2. 현재 경계에 인접한 정점을 반복적으로 탐색
![graph6-4](https://github.com/rungeun/KTC/assets/132816679/62bf839b-d0cd-4783-9310-e29d2440dff9)
![graph6-5](https://github.com/rungeun/KTC/assets/132816679/19d7822e-f38b-4360-a255-f7f69f0ff424)

### 🛠 BFS 코드
```cpp
#include <cstdio>
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int number = 9;     // 노드가 9개
bool c[9];           // 방문 처리를 위한 배열(True or False)
vector<int> a[10];  // 각 노드의 인접 노드를 저장하기 위한 벡터 배열

void bfs(int start) {
    queue<int> q;     // 너비 우선 탐색을 위한 큐 생성
    q.push(start);    // 시작 노드를 큐에 넣음
    c[start] = true;  // 시작 노드를 방문 처리

    while (!q.empty()) { 
        int x = q.front();  // 큐의 맨 앞 노드를 가져옴
        q.pop();            // 큐에서 해당 노드를 제거
        printf("%d ", x);  // 현재 노드 출력

        //  현재 노드와 연결된 인접 노드들을 탐색
        for (int i = 0; i < a[x].size(); i++) {  // a[x].size()는 저장된 값의 개수 반환 (에지의 개수)
            int y = a[x][i];                     // 현재 노드의 인접 노드

            if (!c[y]) {
                q.push(y);    // 방문하지 않은 노드를 큐에 넣음
                c[y] = true;  // 해당 노드를 방문 처리
            }
        }
    }
}

int main(void) {
    a[1].push_back(2);
    a[2].push_back(1);

    a[1].push_back(3);
    a[3].push_back(1);

    a[2].push_back(3);
    a[3].push_back(2);

    a[2].push_back(4);
    a[4].push_back(2);

    a[2].push_back(5);
    a[5].push_back(2);

    a[3].push_back(6);
    a[6].push_back(3);

    a[3].push_back(7);
    a[7].push_back(3);

    bfs(1);
    return 0;
}
```
### BFS 책 코드
```cpp
#include <iostream>
#include <map>
#include <queue>
#include <set>
#include <string>
#include <vector>

using namespace std;

template <typename T>
struct Edge {
    unsigned src;
    unsigned dst;
    T weight;
};

template <typename T>
class Graph {
   public:
    // N개의 정점으로 구성된 그래프
    Graph(unsigned N) : V(N) {}  // 생성자

    // 그래프의 정점 개수 반환
    auto vertices() const { return V; }

    // 전체 에지 리스트 반환
    auto& edges() const { return edge_list; }

    // 정점 v에서 나가는 모든 에지를 반환
    auto edges(unsigned v) const {
        vector<Edge<T>> edges_from_v;
        for (auto& e : edge_list) {
            if (e.src == v) edges_from_v.emplace_back(e);
        }

        return edges_from_v;
    }

    void add_edge(Edge<T>&& e) {
        // 에지 양 끝 정점 ID가 유효한지 검사
        if (e.src >= 1 && e.src <= V && e.dst >= 1 && e.dst <= V)
            edge_list.emplace_back(e);
        else
            cerr << "에러: 유효 범위를 벗어난 정점!" << endl;
    }

    // 표준 출력 스트림 지원
    template <typename U>
    friend ostream& operator<<(ostream& os, const Graph<U>& G);

   private:
    unsigned V;  // 정점 개수
    vector<Edge<T>> edge_list;
};

template <typename U>
ostream& operator<<(ostream& os, const Graph<U>& G) {
    for (unsigned i = 1; i < G.vertices(); i++) {
        os << i << ":\t";

        auto edges = G.edges(i);
        for (auto& e : edges) os << "{" << e.dst << ": " << e.weight << "}, ";

        os << endl;
    }

    return os;
}

template <typename T>
auto create_reference_graph() {
    Graph<T> G(9);

    map<unsigned, vector<pair<unsigned, T>>> edge_map;
    edge_map[1] = {{2, 0}, {5, 0}};
    edge_map[2] = {{1, 0}, {5, 0}, {4, 0}};
    edge_map[3] = {{4, 0}, {7, 0}};
    edge_map[4] = {{2, 0}, {3, 0}, {5, 0}, {6, 0}, {8, 0}};
    edge_map[5] = {{1, 0}, {2, 0}, {4, 0}, {8, 0}};
    edge_map[6] = {{4, 0}, {7, 0}, {8, 0}};
    edge_map[7] = {{3, 0}, {6, 0}};
    edge_map[8] = {{4, 0}, {5, 0}, {6, 0}};

    for (auto& i : edge_map)
        for (auto& j : i.second) G.add_edge(Edge<T>{i.first, j.first, j.second});

    return G;
}

// 주어진 그래프 G에서 너비 우선 탐색(BFS)을 수행하는 함수
template <typename T>
auto breadth_first_search(const Graph<T>& G, unsigned start) {
    queue<unsigned> queue;         // 큐 선언
    set<unsigned> visited;         // 방문한 정점 저장하는 집합
    vector<unsigned> visit_order;  // 방문 순서를 기록하는 벡터
    queue.push(start);             // 시작 정점을 큐에 추가

    // 큐가 비어있을 때까지 반복
    while (!queue.empty()) {
        int current_vertex = queue.front();  // 큐에서 현재 정점을 꺼냄
        queue.pop();

        // 현재 정점을 이전에 방문하지 않았다면
        if (visited.find(current_vertex) == visited.end()) {
            visited.insert(current_vertex);         // 현재 정점을 방문했다고 표시
            visit_order.push_back(current_vertex);  // 방문 순서에 현재 정점을 추가

            // 현재 정점과 연결된 간선들을 순회
            for (auto& e : G.edges(current_vertex)) {
                // 인접한 정점 중에서 방문하지 않은 정점이 있다면 큐에 추가
                if (visited.find(e.dst) == visited.end()) {
                    queue.push(e.dst);
                }
            }
        }
    }

    return visit_order;  // BFS를 마친 후 방문한 정점들의 순서를 반환
}

int main() {
    using T = unsigned;

    // 그래프 객체 생성
    auto G = create_reference_graph<T>();
    cout << "[입력 그래프]" << endl;
    cout << G << endl;

    // 1번 정점부터 BFS 실행 & 방문 순서 출력
    cout << "[BFS 방문 순서]" << endl;
    vector<unsigned> bfs_visit_order = breadth_first_search(G, 1);
    for (auto v : bfs_visit_order) cout << v << endl;
}
```

### 6.2.3 깊이 우선 탐색 DFS
> 시작 정점에서 시작하여 특정 경로를 따라 가능한 멀리 있는 정점을 재귀적으로 먼저 방문하는 방식
#### 과정
1. 가장 먼저 시작 정점부터 방문
![dfs6-8](https://github.com/rungeun/KTC/assets/132816679/36611719-9ef8-46fa-9875-bf453dd30f3a)
2. 이접한 정점 중에서 아직 방문하지 않은 정점을 찾아 탐색을 계속 진행
![dfs6-9](https://github.com/rungeun/KTC/assets/132816679/5fc1720e-9c3e-44b1-8847-74121449e202)
3. 모든 정점을 방문 했다면 탐색 종료
![dsf6-11](https://github.com/rungeun/KTC/assets/132816679/eca53ac7-2273-4ff9-8c36-fc4f8b2b2ead)

### 🛠 DFS 코드
```cpp
#include <iostream>
#include <vector>
using namespace std;

bool visited[9];       // 전역으로 선언하면 모두 false로 초기화 됨.
vector<int> graph[9];  // 벡터 자체가 9개

void dfs(int x) {
    // 현재 노드 방문 처리
    visited[x] = true;
    cout << x << ' ';

    // 그와 연결된 방문하지 않은 노드를 발견할 때마다 재귀호출
    for (int i = 0; i < graph[x].size(); i++) {
        int y = graph[x][i];
        if (!visited[y]) dfs(y);
    }
}

int main(void) {
    // 노드 1에 연결된 노드 정보 저장
    graph[1].push_back(2);
    graph[1].push_back(3);
    graph[1].push_back(8);

    // 노드 2에 연결된 노드 정보 저장
    graph[2].push_back(1);
    graph[2].push_back(7);

    // 노드 3에 연결된 노드 정보 저장
    graph[3].push_back(1);
    graph[3].push_back(4);
    graph[3].push_back(5);

    // 노드 4에 연결된 노드 정보 저장
    graph[4].push_back(3);
    graph[4].push_back(5);

    // 노드 5에 연결된 노드 정보 저장
    graph[5].push_back(3);
    graph[5].push_back(4);

    // 노드 6에 연결된 노드 정보 저장
    graph[6].push_back(7);

    // 노드 7에 연결된 노드 정보 저장
    graph[7].push_back(2);
    graph[7].push_back(6);
    graph[7].push_back(8);

    // 노드 8에 연결된 노드 정보 저장
    graph[8].push_back(1);
    graph[8].push_back(7);

    dfs(1);
}
```

### DFS 책 코드
```cpp
﻿#include <string>
#include <vector>
#include <iostream>
#include <set>
#include <map>
#include <stack>

using namespace std;

template <typename T>
struct Edge
{
	unsigned src;
	unsigned dst;
	T weight;
};

template <typename T>
class Graph
{
public:
	// N개의 정점으로 구성된 그래프
	Graph(unsigned N) : V(N) {}

	// 그래프의 정점 개수 반환
	auto vertices() const { return V; }

	// 전체 에지 리스트 반환
	auto& edges() const { return edge_list; }

	// 정점 v에서 나가는 모든 에지를 반환
	auto edges(unsigned v) const
	{
		vector<Edge<T>> edges_from_v;
		for (auto& e : edge_list)
		{
			if (e.src == v)
				edges_from_v.emplace_back(e);
		}

		return edges_from_v;
	}

	void add_edge(Edge<T>&& e)
	{
		// 에지 양 끝 정점 ID가 유효한지 검사
		if (e.src >= 1 && e.src <= V && e.dst >= 1 && e.dst <= V)
			edge_list.emplace_back(e);
		else
			cerr << "에러: 유효 범위를 벗어난 정점!" << endl;
	}

	// 표준 출력 스트림 지원
	template <typename U>
	friend ostream& operator<< (ostream& os, const Graph<U>& G);

private:
	unsigned V;		// 정점 개수
	vector<Edge<T>> edge_list;
};

template <typename U>
ostream& operator<< (ostream& os, const Graph<U>& G)
{
	for (unsigned i = 1; i < G.vertices(); i++)
	{
		os << i << ":\t";

		auto edges = G.edges(i);
		for (auto& e : edges)
			os << "{" << e.dst << ": " << e.weight << "}, ";

		os << endl;
	}

	return os;
}

template <typename T>
auto create_reference_graph()
{
	Graph<T> G(9);

	map<unsigned, vector<pair<unsigned, T>>> edge_map;
	edge_map[1] = { {2, 0}, {5, 0} };
	edge_map[2] = { {1, 0}, {5, 0}, {4, 0} };
	edge_map[3] = { {4, 0}, {7, 0} };
	edge_map[4] = { {2, 0}, {3, 0}, {5, 0}, {6, 0}, {8, 0} };
	edge_map[5] = { {1, 0}, {2, 0}, {4, 0}, {8, 0} };
	edge_map[6] = { {4, 0}, {7, 0}, {8, 0} };
	edge_map[7] = { {3, 0}, {6, 0} };
	edge_map[8] = { {4, 0}, {5, 0}, {6, 0} };

	for (auto& i : edge_map)
		for (auto& j : i.second)
			G.add_edge(Edge<T>{ i.first, j.first, j.second });

	return G;
}

template <typename T>
auto depth_first_search(const Graph<T>& G, unsigned start)
{
	stack<unsigned> stack;
	set<unsigned> visited;			// 방문한 정점
	vector<unsigned> visit_order;	// 방문 순서
	stack.push(start);

	while (!stack.empty())
	{
		auto current_vertex = stack.top();
		stack.pop();

		// 현재 정점을 이전에 방문하지 않았다면
		if (visited.find(current_vertex) == visited.end())
		{
			visited.insert(current_vertex);
			visit_order.push_back(current_vertex);

			for (auto& e : G.edges(current_vertex))
			{
				// 인접한 정점 중에서 방문하지 않은 정점이 있다면 스택에 추가
				if (visited.find(e.dst) == visited.end())
				{
					stack.push(e.dst);
				}
			}
		}
	}

	return visit_order;
}

int main()
{
	using T = unsigned;

	// 그래프 객체 생성
	auto G = create_reference_graph<T>();
	cout << "[입력 그래프]" << endl;
	cout << G << endl;

	// 1번 정점부터 DFS 실행 & 방문 순서 출력
	cout << "[DFS 방문 순서]" << endl;
	auto dfs_visit_order = depth_first_search(G, 1);
	for (auto v : dfs_visit_order)
		cout << v << endl;
}
```

그래프 순회 문제: [DFS와 BFS 1260번](https://www.acmicpc.net/problem/1260)


### 6.2.5 이분 그래프
서로 인접한 정점끼리 서로 다른 색으로 칠해서 모든 정점을 두가지 색으로만 칠할 수 있는 그래프

![이분 그패프](https://gmlwjd9405.github.io/images/data-structure-graph/bipartite-graph1.gif)

이분 그래프인지 확인하는 방법은 BFS, DFS 탐색을 이용하면 된다.

이분 그래프는 BFS를 할 때 같은 레벨의 정점끼리는 모조건 같은 색으로 칠해진다.

모든 정점을 방문하며 간선을 검사하기 때문에 시간 복잡도는 O(V+E)로 그래프 탐색 알고리즘과 같다.

![이분 그래프](https://gmlwjd9405.github.io/images/data-structure-graph/bipartite-graph2.png)

## 6.3 프림의 최소 신장 트리 알고리즘
1. 가운데 신장 트리는 신장 트리 중 가중치 합이 가장 최소인 최소 신장 트리
2. 신장 트리 중 간선의 가중치 합이 최소인 트리

예제)
- 도시마다 도로를 짓는데 최소 거리가 되도록 구축
- 집마다 전화망 설치하는데 최소 비용이 되도록 설계

![MST](https://velog.velcdn.com/images/suk13574/post/7a9af756-0ab1-457a-82ec-16556318b75b/image.png)


## 최소 신장 트리를 만드는 대표적인 알고리즘 2개
### 크루스칼 알고리즘
그래프의 간선을 하나씩 늘리며 MST를 만든다. 간선을 늘릴 때 가중치가 최소인 간선부터 추가하는 탐욕법을 이용한다

과정
1. 간선은 가중치를 기준으로 오름차순 정렬한다.
2. 간선을 하나씩 살핀다. 간선을 MST에 추가했을 때 MST에 사이클이 생기지 않으면 추가한다. 사이클이 생긴다면 다음 간선으로 넘어간다.

시간 복잡도: O(E log V)

적은 수의 에지로 구성된 희소 그래프에서 사용
### 프림 알고리즘
그래프의 정점 하나씩 늘리며 MST를 만든다. 정점을 늘릴 때 정점과 연결된 간선의 가중치가 최소인 것부터 추가하는 탐욕법을 이용한다.

과정
1. 시작 정점을 고른다. 시작 정점을 MST에 추가한다.
2. 정점과 이어진 간선을 살핀다. 간선과 이어진 다음 정점이 MST에 있지 않다면 이 정점과 간선을 최소 힙에 추가한다.
3. 최소 힙에서 꺼낸 정점이 MST에 포함되어 있지 않다면 MST에 추가하고 과정2를 진행한다. 만약 꺼낸 정점이 MST에 포함되어 있으면 넘어간다.
4. 최소 힙이 빌 때까지 과정3을 반복한다.

시간 복잡도: O(E + V log V)

많은 수의 에지로 구성된 밀집 그래프에서 사용

```cpp
﻿#include <string>
#include <vector>
#include <iostream>
#include <set>
#include <map>
#include <queue>
#include <limits>

using namespace std;

template <typename T>
struct Edge
{
	unsigned src;
	unsigned dst;
	T weight;
};

template <typename T>
class Graph
{
public:
	// N개의 정점으로 구성된 그래프
	Graph(unsigned N) : V(N) {}

	// 그래프의 정점 개수 반환
	auto vertices() const { return V; }

	// 전체 에지 리스트 반환
	auto& edges() const { return edge_list; }

	// 정점 v에서 나가는 모든 에지를 반환
	auto edges(unsigned v) const
	{
		vector<Edge<T>> edges_from_v;
		for (auto& e : edge_list)
		{
			if (e.src == v)
				edges_from_v.emplace_back(e);
		}

		return edges_from_v;
	}

	void add_edge(Edge<T>&& e)
	{
		// 에지 양 끝 정점 ID가 유효한지 검사
		if (e.src >= 1 && e.src <= V && e.dst >= 1 && e.dst <= V)
			edge_list.emplace_back(e);
		else
			cerr << "에러: 유효 범위를 벗어난 정점!" << endl;
	}

	// 표준 출력 스트림 지원
	template <typename U>
	friend ostream& operator<< (ostream& os, const Graph<U>& G);

private:
	unsigned V;		// 정점 개수
	vector<Edge<T>> edge_list;
};

template <typename U>
ostream& operator<< (ostream& os, const Graph<U>& G)
{
	for (unsigned i = 1; i < G.vertices(); i++)
	{
		os << i << ":\t";

		auto edges = G.edges(i);
		for (auto& e : edges)
			os << "{" << e.dst << ": " << e.weight << "}, ";

		os << endl;
	}

	return os;
}

template <typename T>
auto create_reference_graph()
{
	Graph<T> G(9);

	map<unsigned, vector<pair<unsigned, T>>> edge_map;
	edge_map[1] = { {2, 2}, {5, 3} };
	edge_map[2] = { {1, 2}, {5, 5}, {4, 1} };
	edge_map[3] = { {4, 2}, {7, 3} };
	edge_map[4] = { {2, 1}, {3, 2}, {5, 2}, {6, 4}, {8, 5} };
	edge_map[5] = { {1, 3}, {2, 5}, {4, 2}, {8, 3} };
	edge_map[6] = { {4, 4}, {7, 4}, {8, 1} };
	edge_map[7] = { {3, 3}, {6, 4} };
	edge_map[8] = { {4, 5}, {5, 3}, {6, 1} };

	for (auto& i : edge_map)
		for (auto& j : i.second)
			G.add_edge(Edge<T>{ i.first, j.first, j.second });

	return G;
}

template <typename T>
struct Label
{
	unsigned ID;
	T distance;

	// Label 객체 비교는 거리(distance) 값을 이용
	inline bool operator> (const Label<T>& l) const
	{
		return this->distance > l.distance;
	}
};

template <typename T>
auto prim_MST(const Graph<T>& G, unsigned src)
{
	// 최소 힙
	priority_queue<Label<T>, vector<Label<T>>, greater<Label<T>>> heap;

	// 모든 정점에서 거리 값을 최대로 설정
	vector<T> distance(G.vertices(), numeric_limits<T>::max());

	set<unsigned> visited;	// 방문한 정점
	vector<unsigned> MST;	// 최소 신장 트리

	heap.emplace(Label<T>{src, 0});

	while (!heap.empty())
	{
		auto current_vertex = heap.top();
		heap.pop();

		// 현재 정점을 이전에 방문하지 않았다면
		if (visited.find(current_vertex.ID) == visited.end())
		{
			MST.push_back(current_vertex.ID);

			for (auto& e : G.edges(current_vertex.ID))
			{
				auto neighbor = e.dst;
				auto new_distance = e.weight;

				// 인접한 정점의 거리 값이 새로운 경로에 의한 거리 값보다 크면
				// 힙에 추가하고, 거리 값을 업데이트함.
				if (new_distance < distance[neighbor])
				{
					heap.emplace(Label<T>{neighbor, new_distance});
					distance[neighbor] = new_distance;
				}
			}

			visited.insert(current_vertex.ID);
		}
	}

	return MST;
}

int main()
{
	using T = unsigned;

	// 그래프 객체 생성
	auto G = create_reference_graph<T>();
	cout << "[입력 그래프]" << endl;
	cout << G << endl;

	auto MST = prim_MST<T>(G, 1);

	cout << "[최소 신장 트리]" << endl;
	for (auto v : MST)
		cout << v << endl;
	cout << endl;
}
```
## 6.4 다익스트라 최단 경로 알고리즘
>하나의 시작점으로부터 다른 모든 정점까지의 최단거리

다익스트라(dijkstra) 알고리즘은 그래프에서 한 정점(노드)에서 다른 정점까지의 최단 경로를 구하는 알고리즘 중 하나이다. 이 과정에서 도착 정점 뿐만 아니라 모든 다른 정점까지 최단 경로로 방문하며 각 정점까지의 최단 경로를 모두 찾게 된다. 매번 최단 경로의 정점을 선택해 탐색을 반복하는 것이다.


1. 출발 노드와 도착 노드를 설정한다.
2. '최단 거리 테이블'을 초기화한다.
3. 현재 위치한 노드의 인접 노드 중 방문하지 않은 노드를 구별하고, 방문하지 않은 노드 중 거리가 가장 짧은 노드를 선택한다. 그 노드를 방문 처리한다.
4. 해당 노드를 거쳐 다른 노드로 넘어가는 간선 비용(가중치)을 계산해 '최단 거리 테이블'을 업데이트한다.
5. ③~④의 과정을 반복한다.

'최단 거리 테이블'은 1차원 배열로, N개 노드까지 오는 데 필요한 최단 거리를 기록한다. N개(1부터 시작하는 노드 번호와 일치시키려면 N + 1개) 크기의 배열을 선언하고 큰 값을 넣어 초기화시킨다.

'노드 방문 여부 체크 배열'은 방문한 노드인지 아닌지 기록하기 위한 배열로, 크기는 '최단 거리 테이블'과 같다. 기본적으로는 False로 초기화하여 방문하지 않았음을 명시한다.
```cpp
﻿#include <string>
#include <vector>
#include <iostream>
#include <set>
#include <map>
#include <queue>
#include <limits>
#include <algorithm>

using namespace std;

template <typename T>
struct Edge
{
	unsigned src;
	unsigned dst;
	T weight;
};

template <typename T>
class Graph
{
public:
	// N개의 정점으로 구성된 그래프
	Graph(unsigned N) : V(N) {}

	// 그래프의 정점 개수 반환
	auto vertices() const { return V; }

	// 전체 에지 리스트 반환
	auto& edges() const { return edge_list; }

	// 정점 v에서 나가는 모든 에지를 반환
	auto edges(unsigned v) const
	{
		vector<Edge<T>> edges_from_v;
		for (auto& e : edge_list)
		{
			if (e.src == v)
				edges_from_v.emplace_back(e);
		}

		return edges_from_v;
	}

	void add_edge(Edge<T>&& e)
	{
		// 에지 양 끝 정점 ID가 유효한지 검사
		if (e.src >= 1 && e.src <= V && e.dst >= 1 && e.dst <= V)
			edge_list.emplace_back(e);
		else
			cerr << "에러: 유효 범위를 벗어난 정점!" << endl;
	}

	// 표준 출력 스트림 지원
	template <typename U>
	friend ostream& operator<< (ostream& os, const Graph<U>& G);

private:
	unsigned V;		// 정점 개수
	vector<Edge<T>> edge_list;
};

template <typename U>
ostream& operator<< (ostream& os, const Graph<U>& G)
{
	for (unsigned i = 1; i < G.vertices(); i++)
	{
		os << i << ":\t";

		auto edges = G.edges(i);
		for (auto& e : edges)
			os << "{" << e.dst << ": " << e.weight << "}, ";

		os << endl;
	}

	return os;
}

template <typename T>
auto create_reference_graph()
{
	Graph<T> G(9);

	map<unsigned, vector<pair<unsigned, T>>> edge_map;
	edge_map[1] = { {2, 2}, {5, 3} };
	edge_map[2] = { {1, 2}, {5, 5}, {4, 1} };
	edge_map[3] = { {4, 2}, {7, 3} };
	edge_map[4] = { {2, 1}, {3, 2}, {5, 2}, {6, 4}, {8, 5} };
	edge_map[5] = { {1, 3}, {2, 5}, {4, 2}, {8, 3} };
	edge_map[6] = { {4, 4}, {7, 4}, {8, 1} };
	edge_map[7] = { {3, 3}, {6, 4} };
	edge_map[8] = { {4, 5}, {5, 3}, {6, 1} };

	for (auto& i : edge_map)
		for (auto& j : i.second)
			G.add_edge(Edge<T>{ i.first, j.first, j.second });

	return G;
}

template <typename T>
struct Label
{
	unsigned ID;
	T distance;

	// Label 객체 비교는 거리(distance) 값을 이용
	inline bool operator> (const Label<T>& l) const
	{
		return this->distance > l.distance;
	}
};

template <typename T>
auto dijkstra_shortest_path(const Graph<T>& G, unsigned src, unsigned dst)
{
	// 최소 힙
	priority_queue<Label<T>, vector<Label<T>>, greater<Label<T>>> heap;

	// 모든 정점에서 거리 값을 최대로 설정
	vector<T> distance(G.vertices(), numeric_limits<T>::max());

	set<unsigned> visited;					// 방문한 정점
	vector<unsigned> parent(G.vertices());	// 이동 경로를 기억을 위한 벡터

	heap.emplace(Label<T>{src, 0});
	parent[src] = src;

	while (!heap.empty())
	{
		auto current_vertex = heap.top();
		heap.pop();

		// 목적지 정점에 도착했다면 종료
		if (current_vertex.ID == dst)
		{
			cout << current_vertex.ID << "번 정점(목적 정점)에 도착!" << endl;
			break;
		}

		// 현재 정점을 이전에 방문하지 않았다면
		if (visited.find(current_vertex.ID) == visited.end())
		{
			cout << current_vertex.ID << "번 정점에 안착!" << endl;

			// 현재 정점과 연결된 모든 에지에 대해
			for (auto& e : G.edges(current_vertex.ID))
			{
				auto neighbor = e.dst;
				auto new_distance = current_vertex.distance + e.weight;

				// 인접한 정점의 거리 값이 새로운 경로에 의한 거리 값보다 크면
				// 힙에 추가하고, 거리 값을 업데이트함.
				if (new_distance < distance[neighbor])
				{
					heap.emplace(Label<T>{neighbor, new_distance});

					parent[neighbor] = current_vertex.ID;
					distance[neighbor] = new_distance;
				}
			}

			visited.insert(current_vertex.ID);
		}
	}

	// 백트래킹 방식으로 시작 정점부터 목적 정점까지의 경로 구성
	vector<unsigned> shortest_path;
	auto current_vertex = dst;

	while (current_vertex != src)
	{
		shortest_path.push_back(current_vertex);
		current_vertex = parent[current_vertex];
	}

	shortest_path.push_back(src);
	reverse(shortest_path.begin(), shortest_path.end());

	return shortest_path;
}

int main()
{
	using T = unsigned;

	// 그래프 객체 생성
	auto G = create_reference_graph<T>();
	cout << "[입력 그래프]" << endl;
	cout << G << endl;

	auto shortest_path = dijkstra_shortest_path<T>(G, 1, 6);

	cout << endl << "[1번과 6번 정점 사이의 최단 경로]" << endl;
	for (auto v : shortest_path)
		cout << v << " ";
	cout << endl;
}
```
