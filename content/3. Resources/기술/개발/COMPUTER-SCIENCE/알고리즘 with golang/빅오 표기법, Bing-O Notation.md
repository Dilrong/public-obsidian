## 요약

## 내용
### 특징
- [[시간 복잡도]]와 [[공간 복잡도]]를 나타는 방법으로 입력 크기(n)이 커질 때 알고리즘의 실행 시간이나 메모리 사용량이 어떻게 변하는지 표현한다.
- 최악의 경우를 나타나며 성능 비교나 효율성 평가에 중요한 역할을 한다.
### 의미
- 입력 크기에 따라 얼마나 빨리 증가하는지를 나타내기 위해 가장 높은 차수의 항만 고려하여 표현한다.
### 주요 복잡도 유형
1. O(1)
	- 상수 시간 복잡도
	- 알고리즘의 실행 시간이 입력 크기에 영향 받지 않는 경우이다.
```go
func constantTime(arr []int) int {
    return arr[0] // O(1)
}
```
2. O(log n)
	- 로그 시간 복잡도
	- 입력 크기가 증가해도 실행 시간이 완만하게 증가하는 경우로, 주로 이진 탐색에서 발생한다.
```go
func binarySearch(arr []int, target int) int {
    left, right := 0, len(arr)-1
    for left <= right {
        mid := (left + right) / 2
        if arr[mid] == target {
            return mid
        } else if arr[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1 // O(log n)
}
```
## 참고