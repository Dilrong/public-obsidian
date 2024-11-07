## 요약
- 키-값 쌍 형태로 데이터를 빠르게 검색하고 저장할 수 있는 자료구조이다.
- 해시 함수를 이용하여 해시 테이블을 만들고 해시 값을 배열의 인덱스로 변환하여 데이터를 저장하거나 검색한다.
## 내용
### 특징
- 키-값 쌍 형태로 데이터를 빠르게 검색하고 저장할 수 있는 자료구조이다.
- 해시를 활용한 자료구조를 해시 테이블이라고 한다.
	- 키를 해시 함수에 입력하여 얻은 해시 값을 배열의 인덱스로 변환하여 데이터를 저장하거나 검색하는 방식으로 동작한다.
### 핵심 개념
![[image.png]]
- 해시 함수
	- 입력 데이터를 고정된 크기의 해시 값으로 변환하는 함수이다.
- 해시 테이블
	- 해시 함수를 통해 생성된 해시 값을 인덱스로 사용해 데이터를 저장하는 자료 구조이다.
- 버킷
	- 해시 테이블의 인덱스는 버킷이라고 한다.
	- 버킷에는 하나 이상의 데이터가 정될 수 있다.
### 주요 연산
- 삽입
- 검색
- 삭제
### 충돌 해결
- 해시 테이블의 모든 키가 고유한 해시 값을 가질 수 없으므로, 충돌이 발생할 수 있다. 충돌을 해결하기 위한 방법은 다음과 같다.
1. 체이닝
	- 해시 충돌이 발생하면 해당 버킷을 연결 리스트로 구성하여 여러 값을 같은 버킷에 저장할 수 있도록 한다.
	- 검색 시, 버킷 내 연결 리스트를 순회하여 키를 찾는다.
	- ![[image (1).png]]
1. 개방 주소법
	- 충돌이 발생하면 다른 빈 버킷을 찾아 데이터를 저장하는 방식이다.
	- ![[image (2).png]]
	
### 사례
- 데이터베이스 인덱스
- 캐시
- 연관 배열
### 구현체
```go
package main

import "fmt"

type HashTable struct {
	buckets []int
	size int
}

// 해시 테이블 생성
func NewHashTable(size int) *HashTable {
	return &HashTable{
		buckets: make([]int, size),
		size: size,
	}
}

// 해시 함수: 문자열 키를 해시 테이블의 인덱스로 변환
func (h *HashTable) hash(key string) int {
	hashValue := 0
	for _, char := range key {
		// 각 문자의 아스키 값을 더해 해시 값 생성
		hashValue += int(char)
	}
	// 해시 테이블 크기로 나눠 인덱스 생성
	return hashValue % h.size
}

// 키-값 쌍을 해시 테이블에 삽입
func (h *HashTable) Insert(key string, value int) {
	index := h.hash(key)
	h.buckets[index] = value
}

// 키를 통해 값 검색
func (h *HashTable) Search(key string) (int, bool) {
	index := h.hash(key)
	value := h.buckets[index]
	return value, true
}

func main() {
	ht := NewHashTable(10)

    ht.Insert("Alice", 25)
    ht.Insert("Bob", 30)
    ht.Insert("Charlie", 28)

    if value, found := ht.Search("Alice"); found {
        fmt.Println("Alice is value:", value)
    } else {
        fmt.Println("Alice is not found.")
    }

    ht.Insert("Alice", 40)
    if value, found := ht.Search("Alice"); found {
        fmt.Println("After Collision, Alice is value:", value)
    }
}
```
## 참고
- https://go.dev/blog/maps