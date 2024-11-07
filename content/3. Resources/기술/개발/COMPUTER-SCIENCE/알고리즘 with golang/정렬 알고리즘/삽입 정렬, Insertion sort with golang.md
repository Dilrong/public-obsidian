## 요약

## 내용
### 원리
- 이미 정렬된 부분에 새로운 요소를 적절한 위치에  삽입하여 정렬한다.
### 시간 복잡도
- 최악
	- O(n^2)
- 최선은 거의 정렬된 경우 
	- O(n)
### 장점
- 데이터가 적거나 거의 정렬된 경우 매우 효율적이다.
### 단점
- 큰 데이터에서는 비효율적이다.
### 구현체
```go
package main

import "fmt"

func insertionSort(arr []int) {
	n := len(arr)
	for i := 1; i < n; i++ {
		key := arr[i]
		j := i - 1
		for j >= 0 && arr[j] > key {
			arr[j+1] = arr[j]
			j--
		}
		arr[j+1] = key
	}
}

func main() {
	arr := []int{3, 2, 5, 4, 1}

	fmt.Println("Before insertionSort array: ", arr)

	insertionSort(arr)
	fmt.Println("After insertionSort array: ", arr)
}
```
## 참고
- https://visualgo.net/en/sorting