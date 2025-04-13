자바의 컬렉션 프레임워크는 코딩테스트에서 매우 자주 사용되며, 문제 해결의 핵심 도구입니다.

이 문서는 실전에서 자주 쓰이는 컬렉션의 사용법과 팁을 요약한 자료입니다.

---

# 목차

1. [컬렉션 프레임워크 소개](#-컬렉션-프레임워크-소개)
2. [가변 길이 배열](#배열-리스트)
3. [정렬](#정렬)
4. [이진 탐색](#이진-탐색)
3. [스택, 큐](#스택-큐)
4. [집합과 맵](#집합과-맵)
5. [힙](#힙)

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

---

# 가변 길이 배열: ArrayList

## 기본 사용 방법

```java
ArrayList<Integer> arrayList = new ArrayList<>();

// 추가: add
arrayList.add(1); // arrayList: [1]
arrayList.add(2); // arrayList: [1, 2]
arrayList.add(3); // arrayList: [1, 2, 3]

// 조회: get
int get = arrayList.get(1); // get: 2

// 수정: set
arrayList.set(1, 4); // arrayList: [1, 4, 3]

// 삭제: remove
int remove = arrayList.remove(1); // remove: 4, arrayList: [1, 3]
```

---

# 정렬: Arrays.sort, Collections.sort

- 기본 정렬: `void sort(List<T> list)` 메서드 사용
- 커스텀 정렬: `void sort(List<T> list, Comparator<? super T> c)` 메서드 사용

## 정렬 기준 정의: Comparable, Comparator // TODO

자바에서 객체를 정렬하려면 두 가지 인터페이스인 `Comparable`과 `Comparator`를 사용합니다.

- Comparable: 클래스 내부에서 정렬 기준 정의
- Comparator: 클래스 외부에서 정렬 기준 정의

### Comparable, Comparator 기본 사용 방법

#### 예제에서 사용할 Student 클래스

```java
class Student {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

#### Comparable 구현 예제 (이름 오름차순)

```java
// 1. Comparable 인터페이스 구현
class Student implements Comparable<Student> {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    // 2. compareTo 메서드 오버라이드
    @Override
    public int compareTo(Student other) {
        // 이름을 기준으로 비교
        return this.name.compareTo(other.name);
    }
}
```

#### Comparator 구현 예제 (나이 오름차순)

```java
import java.util.Comparator;

// 1. Comparator 인터페이스 구현
public class StudentAgeComparator implements Comparator<Student> {
    // 2. compare 메서드 오버라이드
    @Override
    public int compare(Student student1, Student student2) {
        // 나이를 기준으로 비교
        return student1.getAge() - student2.getAge();
    }
}
```

#### 정렬 예제

```java
Student alice = new Student("Alice", 23);
Student charlie = new Student("Charlie", 21);
Student bob = new Student("Bob", 22);

List<Student> students = new ArrayList<>();
students.add(alice);   // students: [alice]
students.add(charlie); // students: [alice, charlie]
students.add(bob);     // students: [alice, charlie, bob]

// 기본 정렬 (Comparable 기반 정렬)
Collections.sort(students); // students: [alice, bob, charlie]

// 커스텀 정렬 (Comparator 기반 정렬)
StudentAgeComparator studentAgeComparator = new StudentAgeComparator();
Collections.sort(students, studentAgeComparator); // students: [charlie, bob, alice]
```

### Comparable vs Comparator 요약 비교

- Comparable: 클래스 내부에서 정렬 기준 정의 → 기본 정렬 기준 1가지만 정의 가능
- Comparator: 클래스 외부에서 정렬 기준 정의 → 여러 정렬 기준 정의 가능 (`StudentNameComparator`, `StudentAgeComparator` 등)

**_💡 코딩테스트에서 더 많이 쓰는 건?_**

👉 `Comparator` 입니다.

- `Comparable`은 클래스 내부에 정렬 기준을 정의해야 하므로, 수정할 수 없는 외부 클래스(`Integer`, `String` 등)에는 사용 불가
- `Comparator`는 클래스 직접 만들 필요 없이, 람다식으로 바로 작성 가능

아래는 앞서 사용한 `Student` 클래스를 가지고, `Comparator`를 람다식으로 사용하는 예제입니다.

### Comparator 람다 사용 방법

#### 나이 오름차순

```java
Collections.sort(students, (student1, student2) -> {
    return student1.getAge() - student2.getAge();
});
```

또는 더 간단하게

```java
Collections.sort(students, (student1, student2) -> student1.getAge() - student2.getAge());
```

#### 나이 오름차순 + 이름 오름차순

```java
students = getStudents();

Collections.sort(students, (student1, student2) -> {
    if (student1.getAge() != student2.getAge()) {
        return student1.getAge() - student2.getAge();
    }

    return student1.compareTo(student2);
});
```

---

# 이진 탐색: Arrays.binarySearch, Collections.binarySearch

- 기본 정렬 기준 : `int binarySearch(List<? extends Comparable<? super T>> list, T key)` 메서드 사용
- 커스텀 정렬 기준: `int binarySearch(List<? extends T> list, T key, Comparator<? super T> c)` 메서드 사용

## 기본 정렬 기준 // TODO

```java
ArrayList<Integer> arrayList = new ArrayList<>();
arrayList.add(2);
arrayList.add(4);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(8);
arrayList.add(10);

// arrayList: [2, 4, 6, 6, 6, 6, 6, 6, 8, 10]

// 값을 찾은 경우, 찾은 인덱스 반환
int indexOf4 = Collections.binarySearch(arrayList, 4); // 1

// 찾는 값이 여러 개 있는 경우, 일치하는 요소 중 아무 하나의 인덱스만 반환
int indexOf6 = Collections.binarySearch(arrayList, 6); // 2~7 중 하나

// 찾는 값이 없을 경우, -(삽입 포인트) - 1 반환
int indexOf7 = Collections.binarySearch(arrayList, 7); // -9
```

## 커스텀 정렬 기준 // TODO

```java
ArrayList<Integer> arrayList = new ArrayList<>();
arrayList.add(10);
arrayList.add(8);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(6);
arrayList.add(4);
arrayList.add(2);

// arrayList: [10, 8, 6, 6, 6, 6, 6, 6, 4, 2]

// 값을 찾은 경우, 찾은 인덱스 반환
int indexOf4 = Collections.binarySearch(arrayList, 4, Comparator.reverseOrder()); // 8

// 찾는 값이 여러 개 있는 경우, 일치하는 요소 중 아무 하나의 인덱스만 반환
int indexOf6 = Collections.binarySearch(arrayList, 6, Comparator.reverseOrder()); // 2~7 중 하나

// 찾는 값이 없을 경우, -(삽입 포인트) - 1 반환
int indexOf7 = Collections.binarySearch(arrayList, 7, Comparator.reverseOrder()); // -3
```

---

**_⚠️ Arrays.sort, Arrays.binarySearch 사용 시 주의사항 (feat. 원시 자료형)_**

`int[]`, `double[]`, `char[]` 등 원시 자료형 배열은 `Comparator`를 사용할 수 없기 때문에 기본 정렬 기준(오름차순)만 가능

~~커스텀 정렬 기준(내림차순 등)은 `Integer[]`와 같은 래퍼 클래스 배열로 변환~~

---

문제

- [수 정렬하기 2](https://www.acmicpc.net/problem/2751): 오름차순
- [수 정렬하기 4](https://www.acmicpc.net/problem/11931): 내림차순
- [좌표 정렬하기](https://www.acmicpc.net/problem/11650): 커스텀 정렬
- [수 찾기](https://www.acmicpc.net/problem/1920): 이진 탐색 기본 사용

---

# 스택, 큐, 덱: ArrayDeque

- 스택: 후입선출(LIFO)
- 큐: 선입선출(FIFO)
- 덱: 양쪽에서 삽입/삭제 가능 → 스택이나 큐처럼 사용 가능
    - 스택처럼 사용: 뒤에 추가 + 뒤에서 삭제 (앞에 추가 + 앞에서 삭제)
    - 큐처럼 사용: 뒤에 추가 + 앞에서 삭제 (앞에 추가 + 뒤에서 삭제)

## 기본 사용 방법

```java
ArrayDeque<Integer> arrayDeque = new ArrayDeque<>();

// 앞에 추가: addFirst
// 뒤에 추가: addLast
arrayDeque.addFirst(1); // arrayDeque: [   1      ]
arrayDeque.addFirst(2); // arrayDeque: [2, 1      ]
arrayDeque.addLast(3);  // arrayDeque: [2, 1, 3   ]
arrayDeque.addLast(4);  // arrayDeque: [2, 1, 3, 4]

// 앞 조회: getFirst
// 뒤 조회: getLast
int getFirst = arrayDeque.getFirst(); // 2
int getLast = arrayDeque.getLast();   // 4

// 앞에서 삭제: removeFirst
// 뒤에서 삭제: removeLast
int removeFirst = arrayDeque.removeFirst(); // removeFirst: 2, arrayDeque: [1, 3, 4]
int removeLast = arrayDeque.removeLast();   // removeLast: 4, arrayDeque: [1, 3]
```

---

문제

- [스택](https://www.acmicpc.net/problem/10828): 스택 기본 사용
- [큐](https://www.acmicpc.net/problem/10845): 큐 기본 사용
- [덱](https://www.acmicpc.net/problem/10866): 덱 기본 사용

---

# 해시: HashSet, HashMap

## 기본 사용 방법

---

문제

- [문자열 집합](https://www.acmicpc.net/problem/14425): HashSet 기본 사용
- [비밀번호 찾기](https://www.acmicpc.net/problem/17219): HashMap 기본 사용

---

# 트리: TreeSet, TreeMap

## 기본 사용 방법

---

문제

- [회사에 있는 사람](https://www.acmicpc.net/problem/7785): TreeSet 기본 사용
- [파일 정리](https://www.acmicpc.net/problem/20291): TreeMap 기본 사용

---

# 힙: PriorityQueue

## 기본 사용 방법

---

문제

- [최소 힙](https://www.acmicpc.net/problem/1927): 오름차순
- [최대 힙](https://www.acmicpc.net/problem/11279): 내림차순
- [절댓값 힙](https://www.acmicpc.net/problem/11286): 커스텀 정렬

---
