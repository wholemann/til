1. 이해
2. 계획
3. 실행
4. 반성

---

### Linked List Insert & Remove
- 나 자신을 지우거나 내 앞에 무언가 추가하거나는 안 된다. 반드시 내 앞에 노드에서 처리해줘야 한다.
- 내 뒤에 넣는 건 가능하다.
- 맨 앞을 지우거나 추가하려면 HEAD를 조작해야한다. 그래서 List에서 insert, remove 함수를 통해 node를 조작한다.

### Doubly Linked List
- HEAD 외에 LAST TAIL(맨 끝)을 추가하고 각 노드에 prev 를 추가한다.

### STACK
- LIFO
- 이중 링크드 리스트 이용하면 쉬움
- PUSH, POP
- 맨위 : top

### QUEUE
- FIFO
- 이중 링크드 리스트 이용하면 쉬움
- DEQUEUE, ENQUEUE
- 맨앞 : front

### Spike Solution
- 핵심은 완성이 목표가 아님.
- 기술적 검토(뭘 만들려고 하는게 기술적으로 가능한지)
- 막 만들어 보는 것. 자신감을 얻기 위해.

### Prototyping
- 이것도 완성이 목표는 아님.
- 사용자 관점에서 사용가능한지 UX 검증.

### Runner 기법

### Binary Tree
- 순회는 Stack, 재귀를 이용해서 해결할 수 있음.
- Pre-order: Root, Left, Right
- In-order: Left, Root, Right
- Post-order: Left, Right, Root

### Binary Search Tree
- 이상적인 경우 n번만 검색하면 2^n-1 개의 노드를 검색할 수 있음.
- Balanced Tree

### Heap
- Complete Binary Tree: 차근 차근 한 레벨씩 왼쪽부터 채워나가고 해당 레벨의 노드가 다 차면 다음 레벨로 넘어감.
- Min-Heap: 가장 작은 노드를 루트.
- Max-Heap: 가장 큰 노드를 루트.

### Trie(Prefix Tree)
- n개의 children. 알파벳 A-Z(26개).
- 자동 완성 추천.

### Hash Table
- Hash Algorithm: key(String)을 Array의 index(0~n-1)로 바꿔주는 것.

### Hash Collision
- 주어진 데이터 량이 N보다 큼. 비둘기집 원리에 의해 피할 수 없음.
- 공간의 여유가 있지만 변환값이 같음.
- lose lose, djb2, sdbm.

### Separate Chaining & Open Addressing
- 일단 좋은 Hash function을 적용한 뒤에 그럼에도 충돌이 일어날 경우 아래 2가지를 고려한다.
#### Chaining: Array + List(Linked List)
- 각 Node 끼리 string 비교가 일어나기 때문에 되도록 충돌이 적도록 Hash function을 만든 뒤 적용.
#### Open Addressing
- Open Address: 전체 데이터량 <= Array 크기
- 충돌이 일어나지 않는 애들도 옆으로 밀려나는 단점이 있음.

### Set
- 요소 모음(Collection)
- 순서 없음(순서를 보장하지 않는다)
- 중복 없음(유일하다)
- Array, List로 구현. 추가할 때 존재 여부 검사(정렬 해준 뒤).
- HashTable, Tree.(존재 여부 검사의 속도를 향상 시킬 수 있음).

#### Set Operations
- 합집합(union)
- 차집합(difference): 중복되는 부분을 제거.
- 교집합(intersection): 겹치는 걸 찾아냄.
- 부분집합(subset)

### Graph
구성요소
- Node/Vertex
- Edge
- Tree의 일반화된 형태가 Graph

#### Cycle Graph(순환)

순환 Cycle    Acyclic
방향 Directed   Undirected
가중치 Weighted   Unweighted

특수한 형태 : DAG(Directed Acyclic Graph)

#### Graph의 구현

1. Adjacency List(인접 리스트)
2. Adjacency Matrix(인접 행렬)
3. Incidence Matrix

### Shortest Path Problem

