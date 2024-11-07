## 요약
- **고정되고 연속된 값**을 저장하기 위해서 사용하며 삽입, 삭제, 검색이 주요 연산이다.
- 검색은 선형, 이진 탐색을 주로 이용한다.
## 내용
### 특징
- 고정 크기
	- 배열의 크기는 고정되어 있다. 배열을 생성할 때 크기를 지정하면 크기만큼 메모리가 할당된다.
- 동일한 데이터 타입
	- 배열에 저장되는 데이터는 모두 같은 타입이여야 한다.
- 연속된 메모리 공간
	- 배열은 메모리에서 연속된 주소 공간을 차지한다.
### 장점
- 빠른 데이터 접근
	- 인덱스를 통해 즉시 데이터에 접근이 가능하다.
- 간단한 구조
	- 배열은 사용하기 간단하고 이해하기 쉽다.
### 단점
- 크기 제한
	- 크기 변경이 불가능하여, 동적으로 변하는 경우 비효율적이다.
- 삽입 및 삭제 비용
	- 배열 중간에 데이터를 삽입하거나 삭제하려면 나머지 데이터를 이용해야 하므로 처리 비용이 크다.
### 주요 연산
- 삽입
- 삭제
- 검색
	- 주로 선형 탐색 또는 이진 탐색을 이용한다.
### 구현체
```go
package main

import "fmt"

// 선형 탐색
func linearSearch(arr []int, target int) int {
	// 하나씩 탐색
	for i, value := range arr {
		// 원하는 타겟을 찾으면 반환
		if value == target {
			return i
		}
	}
	// 없는 경우
	return -1
}

// 이진 탐색 - 정렬된 경우만 가능
func binarySearch(arr []int, target int) int {
	left, right := 0, len(arr)-1

	for left <= right {
		// 중간 인덱스 계산
		mid := left + (right-left)/2
		// 원하는 타켓을 찾으면 반환
		if arr[mid] == target {
			return mid
		}
		if arr[mid] < target {
			// 타겟이 중간보다 크면 오른쪽 탐색
			left = mid + 1
		} else {
			// 타겟이 중간보다 작으면 왼쪽 탐색
			right = mid - 1
		}
	}
	// 없는 경우
	return -1
}

func main() {
	target := 3
	array := []int{1, 2, 3, 4, 5}

	for i := 0; i < len(array); i++ {
		fmt.Println("index", i, "of value", array[i])
	}

	array[1] = 3
	fmt.Println("array insert and get value", array[1])

	linear_index := linearSearch(array, target)
	fmt.Printf("linear Search (target: %d, index: %d)", target, linear_index)

	binary_index := binarySearch(array, target)
	fmt.Printf("binary SEarch (target: %d, index: %d)", target, binary_index)
}
```
## 참고
- https://go.dev/tour/moretypes/6