# 플로이드 와샬 (Floyd Warshall)

<br>

## 1) 정의

* 모은 노드에서 부터 다른 모든 노드까지의 `최단 경로`를 구하는 알고리즘 (N -> N)
* `음의 가중치`를 가지는 간선이 있어도 `사용 가능`
* `DP` 알고리즘
* 시간 복잡도: O(N^3)
  * 시간 복잡도가 커서 노드 수가 적을 때, 사용하는 것이 좋음
  * 노드의 수가 000개일 경우 사용 가능
  * 노드의 수가 00000개, 간선의 수가 000000개인 경우 다익스트라 사용

<br>

## 2) 점화식

* 각 단계에서 특정 노드를 거치는지 확인
* 노드 a -> b의 최단 거리는 아래와 같이 표현 가능
  * 특정 노드: k

```
Dab = min(Dab, Dak+Dkb)
```

<br>

## 3) 과정

* 초기 최단 거리 테이블을 설정
  * `2차원 배열`을 사용
  * `인접 노드와의 거리`까지 테이블에 저장
  * 다익스트라와 벨만 포드는 1차원 배열을 사용하고 자기 자신은 0, 그 외는 무한으로 저장
* 특정 노드(위의 점화식에서 k)를 지나는 경로를  모든 노드(1~N)에 대해 계산
 
<br>

## 4) 구현

* N개의 노드 (1 ~ N)
* 최단 거리 저장에 `2차원 배열`을 사용

```python
def find_path(start, end):
    if route[start][end] == 0:
        return []

    stop_over = route[start][end] # 경유지
    return find_path(start, stop_over) + [str(stop_over)] + find_path(stop_over, end)

# 노드 수 (1 ~ N)
N = int(sys.stdin.readline())

# 최단 경로의 비용 저장
costs = [[sys.maxsize for i in range(N+1)] for j in range(N+1)]

# 최단 경로 저장
# route[i][j]: j에 도달하기 위해 경유하는 노드 저장 (j 직전에 경유하는 노드)
route = [[0 for i in range(N+1)] for j in range(N+1)]

for i in range(M):
    start, end, cost = map(int, sys.stdin.readline().strip().split())
    costs[start][end] = min(costs[start][end], cost) # start에서 end로 가는 비용은 cost

# 플로이드 와샬
for k in range(1, N+1): # 모든 노드를 경유
    for s in range(1, N+1):
        for e in range(1, N+1):
            if s == e: # 자기 자신으로의 경로는 없음
                costs[s][e] = 0
            else:
                # costs[s][e] = min(costs[s][e], costs[s][k] + costs[k][e])
                if costs[s][e] > costs[s][k] + costs[k][e]:
                    costs[s][e] = costs[s][k] + costs[k][e]
                    route[s][e] = k

# s에서 e까지의 경로
# costs[s][e]가 0 또는 sys.maxsize일 경우, 경로 없음
path = [str(s)] + find_path(s, e) + [str(e)]
```