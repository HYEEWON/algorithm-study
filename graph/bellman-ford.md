# 벨만포드 알고리즘

## 1) 정의
* 특정 노드에서 부터 다른 모든 노드까지의 `최단 경로`를 구하는 알고리즘 (1 -> N)
* `음의 가중치`를 가지는 간선이 있어도 사용 가능
* `음의 사이클` 존재 여부를 알 수 있음
* 다익스트라보다 느림 (벨만포드 시간 복잡도: O(VE))
  * 다익스트라는 인접 노드만 확인
  * 벨만포드는 모든 경우의 수를 탐색룸

## 2) 과정

![bellman-ford](https://user-images.githubusercontent.com/38900338/131061912-738cece4-2f77-4156-91ac-43c3a8449f50.JPG)

- 조건: 노드 N개, 간선 E개
* 1. 출발 노드 설정
* 2. 최단 거리 테이블 초기화
* 3. 아래 과정을 N-1번 반복 (전체 노드 수 - 시작 노드 수 1개)
  * 전체 간선 E개를 하나씩 확인
  * 각 간선을 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블 갱신
* 4. 음의 사이클을 찾을 때에는 3의 과정을 1번 더 수행
  * 테이블이 갱신된다면 음의 사이클이 존재하는 것

## 3) 구현
* 1 ~ N의 노드가 있는 그래프에서의 벨만 포드 알고리즘
* 메서드 'bellmanFord'는 음의 사이클이 있으면 false, 없으면 true 리턴

```java
public static class Node {
        int next;
        int weight;
        public Node(int next, int cost) {
            this.next = next;
            this.weight = cost;
        }
    }

static ArrayList<Node>[] list; // list[x]: 노드 x와 연결된 노드와 가중치의 정보 (new Node(n, w) 저장)
static int[] dist; // 시작점부터 다른 지점까지의 거리
static boolean hasCycle; // 음수 사이클이 없으면 false

public static boolean bellmanFord(int start) {
        dist = new int[N+1];
        Arrays.fill(dist, Integer.MAX_VALUE);

        dist[start] = 0; // 시작점의 거리: 0
        boolean isUpdate = false; // 시간 단축을 위한 변수 (시간 제한이 없으면 사용하지 않아도 됨)

        for(int i = 1; i < N +1; i++) {
            isUpdate = false;

            for(int j = 1; j <= N; j++) {
                for(Node node : list[j]) {
                    if(dist[j] != Integer.MAX_VALUE && dist[node.next] > dist[j] + node.weight) {
                        dist[node.next] = dist[j] + node.weight;
                        isUpdate = true;
                        if (i == N) // N번째에서도 갱신된다면 음의 사이클이 존재하는 것
                            return true;
                    }
                }
            }

            if(!isUpdate) // 갱신이 없다면 종료
                break;
        }

        return false;
    }
```

