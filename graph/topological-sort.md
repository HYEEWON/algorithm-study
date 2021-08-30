# 위상 정렬 (Topological Sort)

* 비순환 방향 그래프(DAG)에서 그래프의 방향성을 거스르지 않고 정점들을 나열하는 것
  * Directed Acyclic Graph: 순환이 없는 방향 그래프
* 결과는 유일하지 않음
* `진입 차수가 0인 순서`로 정렬


## 구현

```python
import sys
from collections import defaultdict, deque

N, M = map(int, sys.stdin.readline().strip().split())

indegree = [0 for i in range(N+1)]
graph = defaultdict(list)
q = deque()

for m in range(M):
    # A가 B보다 앞에 있어야 함
    A, B = map(int, sys.stdin.readline().strip().split())
    indegree[B] += 1
    graph[A].append(B)

for i in range(1, N+1):
    if indegree[i] == 0:
        q.append(i)


while q:
    front = q.popleft()
    sys.stdout.write(str(front) + " ")
    for idx in graph[front]:
        indegree[idx] -= 1
        if indegree[idx] == 0:
            q.append(idx)
```

```java
static int N, M; // 노드 수, 간선 수
static Queue<Integer> q = new LinkedList<>(); // 그래프 순환에 사용, inDegree가 0이면 q에 추가
static int[] inDegree; // 정점을 향하는 노드 수(1~N)
static ArrayList<Integer>[] adjList;

public static void main(String[] args) {
    
    inDegree = new int[N+1];
    adjList = new ArrayList[N+1];
    for(int i=0; i<N+1; i++){
        adjList[i] = new ArrayList<Integer>();
    }

    for (int i = 0; i < M; ++i) {
        // 간선의 방향: a -> b
        inDegree[b]++;
        adjList[a].add(b);
    }

    for (int i = 1; i < N+1; ++i) {
        if (inDegree[i] == 0) q.add(i);
    }

    while(!q.isEmpty()) {
        int front = q.poll();
        System.out.print(front+" "); // 위상 정렬 순서
        for (int i : adjList[front]) { // i: front가 가리키는 정점
            if ((--inDegree[i]) == 0) q.add(i); //진입 차수 -1
        }
    }
}
```