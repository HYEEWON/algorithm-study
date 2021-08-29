# 다익스트라 (Dijkstra)

<br>

## 1) 정의

* 특정 노드에서 부터 다른 모든 노드까지의 `최단 경로`를 구하는 알고리즘 (1 -> N)
* `음의 가중치`를 가지는 간선이 있으면 `사용 불가`
* `그리디` 알고리즘

<br>

## 2) 구현

* 최단 거리 저장에 `1차원 배열`을 사용
* 파이썬: 우선순위 큐로 `heapq` 패키지 사용
* 자바: 우선순위 큐로 `PriorityQueue` 자료구조 사용

```python
import heapq

pq = [] # 우선순위 큐
dist = [sys.maxsize for i in range(N+1)] # 시작점에서 인덱스에 해당하는 노드까지의 거리
isVisit = [False for i in range(N+1)] # 방문 노드를 구별하기 위한 배열 # 방문하면 true

dist[start] = 0

# 우선순위를 위해 가중치를 0번지에 저장
heapq.heappush(pq, (0, start))

while pq:
    cost, cur = heapq.heappop(pq)
    
    if isVisit[cur]:
        continue
    isVisit[cur] = True

    for bus in buses[cur]:
        if (dist[bus.end] > dist[cur] + bus.cost):
            dist[bus.end] = dist[cur] + bus.cost
            heapq.heappush(pq, (dist[bus.end], bus.end))
```

```java
public static class Node implements Comparable<Node> {
    int node;
    int weight;

    public Node(int node, int weight) {
        this.node = node;
        this.weight = weight;
    }

    // 우선순위 큐 사용을 위해 비교 함수 필요
    @Override
    public int compareTo(Node node) {
        return weight - node.weight;
    }
}

static int N; // 노드 수
static ArrayList<Node>[] list; // 노드 저장 (new Node(n, c))
static int[] dist; // 시작점에서 인덱스에 해당하는 노드까지의 거리
static boolean[] isVisit; // 방문 노드를 구별하기 위한 배열 // 방문하면 true
    
public static void dijkstra(int start) {
    boolean[] isVisit = new boolean[N+1];
    dist = new int[N+1];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;

    PriorityQueue<Node> pq = new PriorityQueue<>();
    pq.add(new Node(start, 0));

    while (!pq.isEmpty()) {
        Node cur = pq.poll();

        if (isVisit[cur.node])
            continue;
        isVisit[cur.node] = true;

        for (Node next : list[cur.node]) {
            if (dist[next.node] > dist[cur.node] + next.weight) {
                dist[next.node] = dist[cur.node] + next.weight;
                pq.add(new Node(next.node, dist[next.node]));
            }
        }
    }
}
```