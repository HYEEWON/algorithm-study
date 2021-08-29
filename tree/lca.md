# 최소 공통 조상 (Lowest Common Ancestor, LCA)

## 1) 정의
* 두 노두의 공통된 조상 중에서 가장 가까운 조상
* 아래 트리에서 8과 5의 최소 공통 조상: 2

![lsa](https://user-images.githubusercontent.com/38900338/131098072-709d86aa-974c-41a8-b30a-3a6d5969df36.JPG)


## 2) 과정
- 시간 복잡도: O(lngN)
* 모든 노드의 깊이(depth)를 계산
  * `DFS` 사용
* 두 노드의 깊이가 같아지도록 더 깊은 노드를 거슬러 올라감
* 부모가 같아질 때까지 반복적으로 두 노드의 부모 방향으로 거슬러 올라감
  * 각 노드의 `2^i 번째 부모의 정보`를 저장
  * `DP` 사용

## 3) 구현
* N(1~N)개의 노드를 갖는 트리에서의 LCA 계산

```python
from collections import defaultdict

# 노드들의 깊이 계산, 바로 위의 조상 노드 계산
def dfs(now, depth):
    visit[now] = True
    depths[now] = depth

    for node in tree[now]:
        if visit[node]:
            continue
        parents[node][0] = now
        dfs(node, depth+1)

# parents 배열 갱신
def dp():
    global max_depth
    for i in range(1, max_depth):
        for j in range(1, N+1):
            parents[j][i] = parents[parents[j][i-1]][i-1]

# 최소 공통 조상 찾기
def lca(a, b):
    if depths[a] > depths[b]:
        a, b = b, a

    for depth in range(max_depth-1, -1, -1):
        if depths[parents[b][depth]] >= depths[a]:
            b = parents[b][depth]

    if a == b:
        return a

    for depth in range(max_depth-1, -1, -1):
        if parents[a][depth] != parents[b][depth]:
            a = parents[a][depth]
            b = parents[b][depth]

    return parents[a][0]


# 트리 정의
tree = defaultdict(list)

# 트리의 최대 깊이 계산
tmp = 1;
max_depth = 0;
while tmp <= N: # N: 트리 노드 수
    tmp <<= 1
    max_depth += 1

# dfs() 에서 사용
visit = [False for i in range(N+1)] # 노드 방문 여부 저장
depths = [0 for i in range(N+1)] # 각 노드의 깊이 저장

# parents[v][k]: 정점 v의 2^k번째 조상 번호
parents = [[0 for i in range(max_depth)] for j in range(N+1)]

dfs(root, 1)
dp()

# 노드 a, b의 최소 공통 조상 계산
lca(a, b)
```


```java
static int N; // 노드 수
static ArrayList<Integer>[] list; // 노드들의 연결 상태 저장
static boolean[] visit; // dfs에서 사용
static int[] depths; // 노드들의 깊이 (루트: 1)
static int[][] parents; //parents[v][k]: 정점 v의 2^k번째 조상 번호
static int maxDepth; // 트리의 최대 깊이

public static void main(String[] args) {
    // 트리의 깊이를 구함 (루트: 1)
    int tmp = 1;
    maxDepth = 0;
    while (tmp <= N) {
        tmp <<= 1;
        maxDepth++;
    }

    visit = new boolean[N+1];
    depths = new int[N+1];
    parents = new int[N+1][maxDepth];

    dfs(1, 1);
    dp();

    // 노드 a, b의 최소 공통 조상을 구함
    lca(a, b);

}

// 완전탐색으로 각 노드들의 depth 저장
public static void dfs(int now, int depth) {
    visit[now] = true;
    depths[now] = depth;

    for (int next : list[now]) {
        if (visit[next]) continue;
        parents[next][0] = now;
        dfs(next, depth+1);
    }
}

// DP를 이용해 각 노드별 2^K 번째 조상 노드를 저장
public static void dp() {
    for (int i=1; i<maxDepth; ++i) {
        for (int j=1; j<N+1; ++j) {
            parents[j][i] = parents[parents[j][i-1]][i-1];
        }
    }
}

// 노드 x, y의 최소 공통 조상을 찾는 함수
public static int lca(int x, int y) {
    // 두 노드의 깊이를 동일하게 맞춤
    if (depths[x] > depths[y]) {
        int tmp = x;
        x = y;
        y = tmp;
    }

    for (int i = maxDepth-1; i >= 0; --i) {
        if (depths[parents[y][i]] >= depths[x])
            y = parents[y][i];
    }

    // 두 노드가 같아졌다면 최소 공통 조상
    if (x == y)
        return x;

    // 두 노드를 같은 레벨씩 올림 (2^k 단계)
    for(int i=maxDepth-1; i>=0; i--){
        if(parents[x][i] != parents[y][i]){
            x=parents[x][i];
            y=parents[y][i];
        }
    }

    return parents[x][0];
}
```