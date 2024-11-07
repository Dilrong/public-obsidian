## 요약

## 내용
### 원리
- 힙 트리를 이용하여 정렬한다.
	- 힙 트리는 완전 이진 트리의 일종으로 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰(작은) 이진 트리를 말한다. [[트리, Tree with golang]]
	- 힙 트리는 중복된 값을 허용한다.
### 시간 복잡도
- 모두 O(n log n)
### 장점
- 메모리 사용이 적고 안정적이다.
### 단점
- 데이터가 정렬되는 과정에서 비교 횟수가 많아질 수 있다.
### 구현체
```go
package main

import "fmt"

// 힙 정렬
func heapSort(arr []int) {
	n := len(arr)

	// 최대 힙 만들기
	for i := n/2 - 1; i >= 0; i-- {
		heapify(arr, n, i)
	}

	// 힙에서 요소를 하나씩 제거하여 정렬
	for i := n - 1; i > 0; i-- {
		// 루트와 마지막 요소 교환
		arr[0], arr[i] = arr[i], arr[0]
		// 힙 크기를 줄이고 다시 최대 힙 구조를 만듬
		heapify(arr, i, 0)
	}
}

// 힙 구성
func heapify(arr []int, n, i int) {
	largest := i
	left := 2*i + 1
	right := 2*i + 2

	// 왼쪽 자식이 현재 노드보다 크면 largest 업데이트
	if left < n && arr[left] > arr[largest] {
		largest = left
	}

	// 오른쪽 자식이 largest보다 크면 largest 업데이트
	if right < n && arr[right] > arr[largest] {
		largest = right
	}

	// largest가 i가 아니면 값 교환 후 재귀 호출로 하위 트리 조정
	if largest != i {
		arr[i], arr[largest] = arr[largest], arr[i]
		heapify(arr, n, largest)
	}
}

func main() {
	arr := []int{3, 2, 5, 4, 1}
	fmt.Println("Before heapSort array:", arr)

	heapSort(arr)
	fmt.Println("After heapSort array:", arr)
}
```
## 참고