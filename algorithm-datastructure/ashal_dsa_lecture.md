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

