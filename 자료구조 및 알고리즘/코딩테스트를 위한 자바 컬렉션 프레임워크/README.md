자바의 컬렉션 프레임워크는 코딩테스트에서 매우 자주 사용되며, 문제 해결의 핵심 도구입니다.

이 문서는 실전에서 자주 쓰이는 컬렉션의 사용법과 팁을 요약한 자료입니다.

---

# 자바 컬렉션 프레임워크

자바 컬렉션 프레임워크(Java Collections Framework)는 자료구조와 알고리즘을 포함하는 표준화된 API의 집합으로, 데이터를 효율적으로 저장하고 처리할 수 있도록 도와줍니다.

## 기본 메서드 목록

| 메서드                   | 설명 |
|------------------------------|------|
| `boolean add(E e)`          | 컬렉션에 요소 `e`를 추가 |
| `boolean addAll(Collection<? extends E> c)` | 주어진 컬렉션의 모든 요소를 추가 |
| `boolean remove(Object o)`  | 특정 객체 `o`를 컬렉션에서 제거 |
| `boolean removeAll(Collection<?> c)` | 주어진 컬렉션에 포함된 모든 요소 제거 |
| `boolean contains(Object o)` | 컬렉션에 객체 `o`가 포함되어 있는지 확인 |
| `boolean containsAll(Collection<?> c)` | 주어진 컬렉션의 모든 요소가 포함되어 있는지 확인 |
| `int size()`                | 컬렉션의 요소 개수 반환 |
| `boolean isEmpty()`         | 컬렉션이 비어 있는지 확인 |
| `void clear()`              | 컬렉션의 모든 요소 제거 |

**_💡 팁: CRUD 메서드 네이밍 규칙_**

자바 컬렉션 프레임워크의 대부분의 메서드 네이밍은 **동사(`add`, `get`, `remove` 등) + 대상(`all`, `first`, `last` 등)** 형태의 일관된 규칙을 따르며,

해당 기능이 무엇을 하는지를 직관적으로 알 수 있게 설계되어 있습니다.

## 주요 구현 클래스 목록

- ArrayList: 가변 길이 배열
- ArrayDeque: 스택, 큐, 덱
- PriorityQueue: 우선순위 큐
- HashSet: 중복 제거
- HashMap: 중복 제거
- TreeSet: 중복 제거 + 정렬
- TreeMap: 중복 제거 + 정렬

---
