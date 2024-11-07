## 요약
- 먼저 들어온 데이터가 먼저 나가는 자료구조이다.
## 내용
### 특징
- 선입선출(FIFO) 방식의 자료구조이다.
- 먼저 들어온 데이터가 먼저 나가는 구조이다.
- 데이터가 들어가는 뒤쪽(Rear), 나가는 앞쪽(Front)만 있다.
### 주요 연산
- 삽입 (Enqueue)
- 삭제 (Dequeue)
- 확인 (Peek)
### 종류
- 일반 큐
- 원형 큐
	- [[연결 리스트, Linked List with golang]]의 원형 연결 리스트와 동일하게 마지막 요소가 첫번째 요소와 연결되어 공간을 절약한다.
- 우선순위 큐
	- FIFO구조 대신, 요소에 우선순위를 부여하여 높은 우선순위 요소가 먼저 처리된다.
- 이중 큐
	- 양쪽에서 삽입과 삭제가 가능한 큐이다.
### 사례
- 프로세스 스케줄링
- 프린터 대기열
- 데이터 스트리밍
### 구현체
```go
package main

import "fmt"

type Queue struct {
	elements []int
}

// Enqueue: 큐의 뒤에 요소 추가
func (q *Queue) Enqueue(value int) {
	q.elements = append(q.elements, value)
}

// Dequeue: 큐의 앞에서 요소 제거 및 반환
func (q *Queue) Dequeue() (int, bool) {
	// 큐가 비어있다면
	if len(q.elements) == 0 {
		return 0, false
	}
	value := q.elements[0]
	// 앞쪽 요소 제거
	q.elements = q.elements[1:]
	return value, true
}

// Peek: 큐의 앞쪽 요소 확인
func (q *Queue) Peek() (int, bool) {
	// 큐가 비어있다면
	if len(q.elements) == 0 {
		return 0, false
	}
	return q.elements[0], true
}

// IsEmpty: 큐가 비어있는지 확인
func (q *Queue) IsEmpty() bool {
	return len(q.elements) == 0
}

func main() {
	q := Queue{}

	q.Enqueue(1)
	q.Enqueue(2)
	q.Enqueue(3)

	fmt.Println("Queue Status: ", q.elements)

	if front, ok := q.Peek(); ok {
		fmt.Println("Peek:", front)
	}

	for !q.IsEmpty() {
		value, _ := q.Dequeue()
		fmt.Println("Dequeue:", value)
	}

	fmt.Println("Is queue empty?", q.IsEmpty())
}
```

## 참고