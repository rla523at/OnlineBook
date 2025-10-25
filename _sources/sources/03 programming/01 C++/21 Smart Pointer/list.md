# list

* https://en.cppreference.com/w/cpp/container/list
  * std::list is a container that supports constant time insertion and removal of elements from anywhere in the container.
  * Fast random access is not supported
  * Compared to std::forward_list this container provides bidirectional iteration capability while being less space efficient.

## erase
* https://en.cppreference.com/w/cpp/container/list/erase
  * References and iterators to the erased elements are invalidated. Other references and iterators are not affected.

## forward list
| 특징                         | std::list                                  | std::forward_list                           |
|------------------------------|--------------------------------------------|---------------------------------------------|
| **연결 방식**                | 이중 연결 리스트 (doubly-linked list)      | 단일 연결 리스트 (singly-linked list)         |
| **반복자(Iterator) 지원**      | 양방향 반복자 (bidirectional iterator)     | 단방향 반복자 (forward iterator)             |
| **메모리 오버헤드**          | 노드 당 이전 노드 포인터와 다음 노드 포인터가 필요하므로 메모리 오버헤드가 큼 | 노드 당 다음 노드 포인터만 필요하므로 메모리 사용량이 적음 |
| **인터페이스**               | push_front, push_back, pop_front, pop_back, insert, erase 등 다양한 조작 기능 제공 | push_front, pop_front, insert_after, erase_after 등 제한적인 인터페이스 제공 |
| **size() 메서드**            | 상수 시간(O(1))에 구현되는 경우가 많음        | C++11 표준에서는 size()가 제공되지 않거나 선형 시간(O(n))일 수 있음 |
