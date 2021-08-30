# 최소 신장 트리 (Minimum Spanning Tree, MST)

## 1) 정의
* 신장 트리 중, `간선의 가중치의 합이 최소`인 트리
* 신장 트리: 모든 노드가 연결되어 있는 트리, 간선에 방향성이 없고 사이클이 존재하지 않음 
* `유니온 파인트`를 이용해 구할 수 있음 -> 간선의 가중치를 기준으로 오름차순 정렬한 뒤, 유니온 파인드 실행

<br>

## 2) 크루스칼 알고리즘

- `그리디` 알고리즘

* 1. 간선을 `오름차순`으로 정렬
* 2. 현재의 간선이 사이클을 발생시키는지 확인
  * 사이클이 없는 경우: MST에 포함
  * 사이클이 있는 경우: MST에 포함하지 않음
* 3. 모든 간선에 대해 2를 반복

```python
class Edge:
    def __init__(self, start, end, cost):
        self.start = start
        self.end = end
        self.cost = cost

def find(x):
    if parents[x] == x:
        return x
    else:
        parents[x] = find(parents[x])
        return parents[x]

# 정점의 수 (1~V), 간선의 수
V, E

# 부모 노드 저장
parents = [i for i in range(V+1)]

# 간선 배열, Edge(start, end, cost) 저장
edges = []

# 가중치를 기준으로 오름차순 정렬
edges = sorted(edges, key=lambda x: x.cost)

# 가중치 합을 구함
sum_cost = 0
for edge in edges:
    s = find(edge.start)
    e = find(edge.end)
    
    # 루트가 같다면 MST가 아님
    if s == e:
        continue

    # 루트가 다르다면 합침
    parents[e] = s
    sum_cost += edge.cost
```
## 3) 프림 알고리즘