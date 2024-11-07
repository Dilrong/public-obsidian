## 요약
- 노드 단위로 연결된 리스트로 노드에 데이터와 다음 **노드 포인터**를 가지고 있다.
- 배열과 유사하지만 가변적이고 연속적일 필요가 없다.
## 내용
### 특징
- 노드라는 개별 요소들이 연결된 형태의 자료구조이다.
- 노드는 데이터를 담는 부분과 다음 노드를 가리키는 포인터를 포함한다.
- 메모리 공간이 연속적이지 않아도 되기 때문에 삽입 및 삭제가 용이하다.
### 구조
- 노드 (Node)
	- 연결 리스트의 기본 단위로 데이터와 포인터를 포함한다.
- 헤드 (Head)
	- 연결 리스트의 첫 번째 노드를 가리키는 포인터이다. 리스트를 탐색하려면 헤드부터 시작해야 한다.
- 포인터 (Pointer): 각 노드가 다음 노드를 가리키는 역할을 한다.
### 종류
1. 단일 연결 리스트
	- 각 노드가 다음 노드에 대한 포인터만 가지는 연결 리스트이다.
2. 이중 연결 리스트
	- 각 노드가 이전 노드와 다음 노드를 가리키는 두 개의 포인터를 가지는 연결 리스트이다.
3. 원형 연결 리스트
	- 마지막 노드가 첫 번째 노드를 가리키는 순환 구조의 연결 리스트
### 주요 연산
- 삽입
- 삭제
- 검색
- 출력
### 구현체
```go
package main

import "fmt"

type Node struct {
	data int
	next *Node
}

type LinkedList struct {
	head *Node
}

// 삽입
func (list *LinkedList) Append(data int) {
	newNode := &Node{data: data, next: nil}
	if list.head == nil {
		// 헤드가 없는 경우
		list.head = newNode
	} else {
		current := list.head
		// 마지막 노드를 찾는다.
		for current.next != nil {
			current = current.next
		}
		current.next = newNode
	}
}

// 삭제
func (list *LinkedList) Delete(data int) {
	if list.head == nil {
		return
	}

	// 삭제할 값이 헤드인 경우
	if list.head.data == data {
		list.head = list.head.next
		return
	}
	// 삭제할 노드를 찾는다.
	current := list.head
	for current.next != nil {
		if current.next.data == data {
			current.next = current.next.next
			return
		}
		current = current.next
	}
}

// 검색
func (list *LinkedList) Search(data int) bool {
	current := list.head
	for current != nil {
		if current.data == data {
			return true
		}
		current = current.next
	}
	return false
}

// 출력
func (list *LinkedList) Display() {
	current := list.head
	for current != nil {
		fmt.Print(current.data, " ")
		current = current.next
	}
	fmt.Println()
}

func main() {
	list := LinkedList{}

	list.Append(1)
	list.Append(2)
	list.Append(3)
	list.Append(4)
	list.Append(5)

	fmt.Print("Linked List: ")
	list.Display()

	fmt.Println("Does list has a 3 ?", list.Search(3))

	list.Delete(4)
	fmt.Println("After delete, list: ")
	list.Display()
}
```

## 참고