## 요약

## 내용
### 원리
- 배열을 절반으로 나누어 각각 정렬한 후 병합하는 분할 정복 알고리즘이다.
### 시간 복잡도
- 모두 O(n log n)
### 장점
- 안정적이고 ,큰 데이터에 효율적이다.
### 단점
- 추가적인 메모리가 필요하여 메모리 사용에 문제가 될 수 있다.
### 구현체
```go
package main

import "fmt"

func mergeSort(arr []int) []int {
	if len(arr) <= 1 {
		return arr
	}
	mid := len(arr) / 2
	left := mergeSort(arr[:mid])
	right := mergeSort(arr[mid:])
	return merge(left, right)
}

func merge(left, right []int) []int {
	result := []int{}
	i, j := 0, 0

	// 두 배열을 비교하며 작은 값을 결과 배열에 추가
	for i < len(left) && j < len(right) {
		if left[i] < right[j] {
			result = append(result, left[i])
			i++
		} else {
			result = append(result, right[j])
			j++
		}
	}
	result = append(result, left[i:]...)
	result = append(result, right[j:]...)
	return result
}

func main() {
	arr := []int{3, 2, 5, 4, 1}

	fmt.Println("Before mergeSort array: ", arr)

	sortedArr := mergeSort(arr)
	fmt.Println("After mergeSort array: ", sortedArr)
}
```
## 참고
- https://visualgo.net/en/sorting