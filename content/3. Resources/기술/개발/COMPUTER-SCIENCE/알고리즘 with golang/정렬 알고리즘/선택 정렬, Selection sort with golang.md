## 요약
- 배열에서 가장 작은 요소를 찾아 맨 앞 요소와 교환하며 정렬하는 알고리즘이다.
## 내용
### 원리
- 배열에서 가장 작은 요소를 찾아 맨 앞 요소와 교환하며 정렬한다.
### 시간 복잡도
- 모두 O(n^2)
### 장점
- [[버블 정렬, Bubble sort with golang]]보다 비교 횟수가 적고, 데이터가 적은 경우 간단하게 사용 가능하다.
### 단점
- 데이터 양이 많아질수록 비효율적입니다.
### 구현체
```go
package main

import "fmt"

func selectionSort(arr []int) {
	n := len(arr)
	for i := 0; i < n-1; i++ {
		minIdx := i
		for j := i + 1; j < n; j++ {
			if arr[j] < arr[minIdx] {
				minIdx = j
			}
		}
		arr[i], arr[minIdx] = arr[minIdx], arr[i]
	}
}

func main() {
	arr := []int{3, 2, 5, 4, 1}

	fmt.Println("Before selectionSort array: ", arr)

	selectionSort(arr)
	fmt.Println("After selectionSort array: ", arr)
}
```
## 참고
- https://visualgo.net/en/sorting