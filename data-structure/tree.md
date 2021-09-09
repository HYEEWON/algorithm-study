# 트리

## 1) 정의

* `사이클을 가지지 않는` 그래프
* 부모-자식 관계로 `계층적` 구조를 갖는 자료구조

## 2) 용어

* 루트: 트리의 최상위 노드
* 형제 노드: 부모가 동일한 노드
* 조상 노드: 특정 노드에서 루트까지 가는 경로의 모든 노드
* 후손 노드: 특정 노드를 루트로 하는 서브 트리의 모든 노드
* 깊이: 루트부터 특정 노드 사이의 간선 수
  * 루트의 깊이: 0
* 레벨: 깊이 + 1
* 트리의 높이: 트리에서 가장 큰 레벨
* 트리의 차수: 자식 노드의 수의 최대 값

## 3) 구현

![tree](https://user-images.githubusercontent.com/38900338/130806325-95537a03-fe8f-488f-807c-5fe80aa34e23.JPG)

* 배열의 인덱스는 `1에서 시작`
* 노드 N -> 왼쪽 자식 노드: 2 * N, 오른쪽 자식 노드: 2 * N + 1

<br>

# 이진 트리 (Binary Tree)

## 1) 정의

* 최대 2개의 자식을 가지는 트리 (차수가 최대 2)
* 높이 H: -> `최대 노드 수: 2^H - 1`

## 2)종류

* 포화 이진 트리: 각 레벨의 노드가 꽉 차있는 트리, 모든 단말 노드의 깊이가 같음
* 완전 이진 트리: 높이가 K인 트리에서 레벨 1~K-1까지 모두 채워져 있고 마지막 레벨에서는 왼쪽부터 순서대로 채워져 있는 트리

## 3) 순회

* 전위 순회 (pre-order): 루트 -> 왼쪽 -> 오른쪽
* 중위 순회 (in-order): 왼쪽 -> 루트 -> 오른쪽
* 후위 순회 (post-order): 왼쪽 -> 오른쪽 -> 루트

<br>

# 힙 (Heap)

## 1) 정의

* `이진 트리`의 응용
* 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰/작은 `완전 이진 트리`
* `우선 순위 큐`를 구현하는 자료구조로 `최소값`과 `최대값`을 쉽게 알 수 있음
* `힙 정렬`에 사용

## 2) 종류

* 최소 힙: 부모 노드의 키 값이 자식 노드의 키 값보다 항상 작음
* 최대 힙: 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큼

## 3) 삽입과 삭제  

* 시간 복잡도: O(logN)
* 삽입: 트리의 가장 마지막에 삽입하여 힙 조건을 만족할 때까지 부모와 자식을 교환
* 삭제: 루트를 삭제하고 트리의 가장 마지막 노드를 루트로 삽입, 힙 조건을 만족할 때까지 부모와 자식을 교환

<br>

# 우선순위 큐 (Priority Queue)

## 1) 정의

* `힙`의 활용
* 데이터에 우선순위를 부여하여 우선순위가 높은 데이터부터 꺼내는 자료구조
* 우선순위는 `Heap`을 통해 구현
* 우선순위를 정해 데이터를 추가하고, 우선순위가 가장 높은 데이터를 삭제할 수 있음

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

<br>

# 세그먼트 트리 (Segment Tree)

## 1) 정의

* `이진 트리`의 응용
* 리프 노드들은 배열의 값 자체를 가지고 있음
* `구간 합`, `구간의 최대/최소값`을 구할 때 사용
* 합 트리: 이 외의 노드들은 자식 노드 값들의 합을 저장
* 최대/최소 값 트리: 이 외의 노드들은 자식 노드 값 중의 최대/최소 값을 저장

## 2) 구현

![segment_tree](https://user-images.githubusercontent.com/38900338/130806208-3fcebb27-0f98-412d-9d0b-d76d2095da40.JPG)

```java
public class Main {
    public static void main(String[] args) {
        // 배열은 1번지부터 사용
        int[] array = new long[N+1];
        long[] tree = new long[N*4];

        init(1, 1, N);
        query(1, 1, N, srchFrom, srchTo);
        update(1, 1, N, idx, val);
    }

    // 트리 초기화
    public static long init(int node, int from, int to) {
		if (from == to) return tree[node] = array[from];
		int mid = (from+to) / 2;
		
		return tree[node] = init(node*2, from, mid) + init(node*2+1, mid+1, to);
	}
	
    // idx번째 수를 val로 변경
	public static long update(int node, int from, int to, int idx, long val) {
		if (from > idx || to < idx) return tree[node];
		if (from == to) return tree[node] = val;
		int mid = (from+to) / 2;
		
		return tree[node] = update(node*2, from, mid, idx, val) + update(node*2+1, mid+1, to, idx, val);
	}
	
    // srchFrom ~ srchTo 구간의 합을 구함
	public static long query(int node, int from, int to, int srchFrom, long srchTo) {
		if (from > srchTo || to < srchFrom) return 0;
		if (from >= srchFrom && to <= srchTo) return tree[node];
		
		int mid = (from+to) / 2;
		
		return query(node*2, from, mid, srchFrom, srchTo) + query(node*2+1, mid+1, to, srchFrom, srchTo);
	}
}
```

<br>

# 트라이 (Trie)

## 1) 정의

* 문자열 검색을 위한 `이진 검색 트리`
* 노드에 `한 문자`씩 저장

## 2) 삽입
* 루트는 비워져 있음
* 추가하려는 문자열의 첫글자가 루트의 자식이라면 탐색, 없다면 첫글자를 자식으로 추가
* 동일한 문자가 있다면 다음 글자로 넘어감, 동일한 문자가 없으면 새로운 문자를 자식으로 추가
* 반복

## 3) 탐색
* 루트의 자식부터 탐색 시작
* 자식 아래로 탐색 계속
* 문자열의 길이 == 리프 레벨 -> 검색 성공

## 4) 구현

![trie](https://user-images.githubusercontent.com/38900338/130806414-e7586e7a-4111-4131-ba19-44cf77ec0ab9.JPG)

```python
class Node(object):
    def __init__(self, key, end = None):
        self.key = key
        self.end = end
        self.child = {}

class Trie:
    def __init__(self):
        self.head = Node(self)
    
    def insert(self, word):
        cur = self.head

        for w in word:
            if w not in cur.child:
                cur.child[w] = Node(w)
            cur = cur.child[w]
        cur.end = True

    def search(self, word):
        cur = self.head

        for w in word:
            if w in cur.child:
                cur = cur.child[w]
            else:
                return False

        if cur.end == True:
            return True

trie = Trie()
trie.insert(word)
trie.update(word)
```
