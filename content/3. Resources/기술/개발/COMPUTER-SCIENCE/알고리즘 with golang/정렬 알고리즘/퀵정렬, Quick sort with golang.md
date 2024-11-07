## 요약

## 내용
### 원리
- 피벗(pivot)을 중심으로 작은 값은 왼쪽, 큰 값은 오른쪽으로 분할하여 정렬하는 분할 정복 알고리즘이다.
- 작은 값은 왼쪽, 큰 값은 오른쪽에 위치하도록 배열을 재정렬한다.
- 재귀로 호출하고 부분 배열의 크기가 1이 되면, 전체 배열이 정렬된 상태가 된다.
### 시간 복잡도
- 평균
	- O(n log n)
- 최악, 정렬된 배열에서 첫 번째 요소를 피벗으로 선택한 경우
	- O(n^2)
### 장점
- 추가 메모리 사용이 적고, 평균적으로 빠르다.
### 단점
- 최악의 경우 시간 복잡도가 높을 수 있다.
### 구현체
```go
package main

import "fmt"

func quickSort(arr []int, low, high int) {
	if low < high {
		pi := partition(arr, low, high)
		quickSort(arr, low, pi-1)
		quickSort(arr, pi+1, high)
	}
}

func partition(arr []int, low, high int) int {
	pivot := arr[high]
	i := low - 1
	for j := low; j < high; j++ {
		if arr[j] <= pivot {
			i++
			arr[i], arr[j] = arr[j], arr[i]
		}
	}
	arr[i+1], arr[high] = arr[high], arr[i+1]
	return i + 1
}

func main() {
	arr := []int{3, 2, 5, 4, 1}

	fmt.Println("Before quickSort array: ", arr)

	quickSort(arr, 0, len(arr)-1)
	fmt.Println("After quickSort array: ", arr)
}
```
## 참고
- https://visualgo.net/en/sorting