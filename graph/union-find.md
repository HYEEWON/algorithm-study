# 유니온 파인드 (서로소 집합, Disjoint Set)

## 1) 정의

* 서로소 집합(교집합이 공집합인 집합)의 정보를 저장하는 자료구조
* 두 노드가 같은 그래프에 속하는지 판별할 수 있음
* 원소/집합을 합하는 `Union` 함수와 `Find` 함수로 구성
* 계속 합치거나 분리하는 경우, 그룹의 멤버 수를 구하는 경우 사용

<br>

## 2) 구성

* `Union(x, y)`: 두 원소 x, y가 속한 집합을 하나로 합침 (루트를 일치시킴)
* `Find(x)`: x가 속한 집합을 찾음 (루트를 찾음)
* 일반적으로 작은 값을 루트로 함
* `Root` 노드는 집합을 구분하는 ID 처럼 생각

![union-find](https://user-images.githubusercontent.com/38900338/131327601-b618e6be-eb3b-4f80-aa05-d1a48af30c07.JPG)

<br>

## 3) 구현

``` python
#parent[i]: i의 부모 노드, 노드의 부모 노드 저장 
parents = [i for i in range(N+1)]

# x의 Root 노드를 반환
def find(x):
    if parents[x] == x: # 루트이면 반환
        return x
    else:
        parents[x] = find(parents[x])
        return parents[x]

# x, y원소가 속합 집합을 합치는 함수
def union(x, y):
    x = find(x)
    y = find(y)
    if (x != y):
        parent[y] = x # y의 Root를 x로 함

# 높이가 작은 트리가 높이가 큰 트리 아래로 추가
# Rank: 트리의 높이를 저장
def union(x, y):
    x = find(x)
    y = find(y)
    if (x != y):
        parent[y] = x
    
    if rank[x] > rank[y]:
        parent[y] = x;
        rank[x] += rank[y]
    else:
        parent[x] = y
        rank[y] += rank[x]
```