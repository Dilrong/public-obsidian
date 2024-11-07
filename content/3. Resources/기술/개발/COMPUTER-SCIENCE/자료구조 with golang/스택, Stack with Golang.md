## 요약
- 선입후출, 마지막에 추가된 데이터가 가장 먼저 제거되는 방식의 자료 구조이다.
## 내용
### 특징
- 선입후출(LIFO)구조의 자료 구조이다.
- 마지막에 추가된 데이터가 가장 먼저 제거되는 방식이다.
### 주요 연산
- 삽입(Push)
- 삭제(Pop)
- 확인(Peek)
### 사례
- 함수 호출 관리
	- [[스택 프레임]]
- 되돌리기
### 구현체
```go
package main

import "fmt"

type Stack struct {
	elements []int
}

// Push: 요소 추가
func (s *Stack) Push(value int) {
	s.elements = append(s.elements, value)
}

// Pop: 스택에서 요소 제거 및 반환
func (s *Stack) Pop() (int, bool) {
	// 스택이 비어있는 경우
	if len(s.elements) == 0 {
		return 0, false
	}
	// 맨 위 요소 저장
	value := s.elements[len(s.elements)-1]
	// 맨 위 요소 제거
	s.elements = s.elements[:len(s.elements)-1]
	return value, true
}

// Peek: 맨 위 요소 확인
func (s *Stack) Peek() (int, bool) {
	if len(s.elements) == 0 {
		return 0, false // 스택이 비어있는 경우
	}
	return s.elements[len(s.elements)-1], true
}

// IsEmpty: 스택이 비어있는지 확인
func (s *Stack) IsEmpty() bool {
	return len(s.elements) == 0
}

func main() {
	s := Stack{}

	s.Push(1)
	s.Push(2)
	s.Push(3)

	fmt.Println("Stack Status", s.elements)

	if top, ok := s.Peek(); ok {
		fmt.Println("Peek:", top)
	}

	for !s.IsEmpty() {
		value, _ := s.Pop()
		fmt.Println("Pop:", value)
	}

	fmt.Println("Is stack empty?", s.IsEmpty())
}
```
- 배열을 참조하는 자료 구조인 Slice를 사용하여 구현
## 참고