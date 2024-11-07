## 요약
- 인접한 두 요소를 비교하여 정렬이 완료될 때까지 반복하는 알고리즘이다.
## 내용
### 원리
- 인접한 두 요소를 비교하여 정렬이 완료될 때까지 반복한다.
- 큰 값이 배열이 끝으로 "거품"처럼 이동하기 때문에 버블 정렬이라고 불린다.
	- 물 속의 공기가 물 표면 위로 떠오르는것 처럼 올라가기 때문이다.
### 시간 복잡도
- 최악 및 평균
	- O(n^2)
- 최선은 이미 정렬된 경우
	- O(n)
### 장점
- 구현이 간단하다
- 데이터 양이 적거나 거의 정렬된 경우 유리하다
### 단점
- 다른 정렬 알고리즘에 비해 속도가 느리고 효율이 낮다.
### 구현체
```go
package main

import "fmt"

func bubbleSort(arr []int) {
	n := len(arr)
	for i := 0; i < n-1; i++ {
		for j := 0; j < n-i-1; j++ {
			if arr[j] > arr[j+1] {
				arr[j], arr[j+1] = arr[j+1], arr[j]
			}
		}
	}
}

func main() {
	arr := []int{3, 2, 5, 4, 1}

	fmt.Println("Before bubbleSort array: ", arr)

	bubbleSort(arr)
	fmt.Println("After bubbleSort array: ", arr)
}

```
## 참고
- https://visualgo.net/en/sorting