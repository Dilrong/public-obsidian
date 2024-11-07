## 요약

## 내용
### 특징
- 정점(Vertex)와 간선(Edge)으로 구성된 자료 구조로 간선을 통해 다른 노드와 연결되는 비선형 자료구조이다.
### 핵심 개념
- 정점(Vertex)
	- 그래프에서 개별 객체를 의미한다.
	- 데이터를 저장한다.
- 간선(Edge)
	- 두 정점을 연결하는 관계나 경로를 나타낸다.
- 가중치(Weight)
	- 간선에 부여된 값으로, 경로의 비용, 거리 등을 나타낼 수 있다.
	- 가중치가 있는 그래프는 가중 그래프라고 부른다.
### 주요 연산
- 삽입
- 삭제
- 탐색
	- 너비 우선 탐색(BFS) 또는 깊이 우선 탐색(DFS)이 자주 사용된다.
### 종류
- 방향 그래프
	- 간선이 한 방향으로만 연결된 그래프이다.
- 무방향 그래프
	- 간선이 양방향으로 연결된 그래프이다.
- 가중 그래프
	- 간선에 가중치가 있는 그래프이다.
- 비가중 그래프
	- 간선에 가중치가 없는 그래프이다.
- 연결 그래프
	- 모든 정점이 연결된 그래프이다.
- 비연결 그래프
	- 모든 정점이 연결되지 않은 그래프이다.
### 예시
- 네트워크 경로 찾기
- 소셜 네트워크 분석
- 경로 탐색
### 구현체
```go
package main

import "fmt"

type Graph struct {
	// 정점과 연결된 정점들의 리스트
	vertices map[int][]int
}

// 새로운 그래프 생성
func NewGraph() *Graph {
	return &Graph{vertices: make(map[int][]int)}
}

// 간선 추가, 무방향 그래프
func (g *Graph) AddEdge(v1, v2 int) {
	g.vertices[v1] = append(g.vertices[v1], v2)
	g.vertices[v2] = append(g.vertices[v2], v1)
}

// 정점 출력
func (g *Graph) Display() {
	for vertex, edges := range g.vertices {
		fmt.Printf("정점 %d: %v\n", vertex, edges)
	}
}

func main() {
	g := NewGraph()

	g.AddEdge(1, 2)
	g.AddEdge(1, 3)
	g.AddEdge(2, 4)
	g.AddEdge(3, 4)
	g.AddEdge(4, 5)

	fmt.Println("Graph display:")
	g.Display()
}
```

## 참고