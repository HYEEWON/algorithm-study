# 세그먼트 트리 (Segment Tree)

## 1) 정의

* `이진 트리`의 응용
* 리프 노드들은 배열의 값 자체를 가지고 있음
* `구간 합`, `구간의 최대/최소값`을 구할 때 사용
* 포화 이진 트리가 아니어도 됨
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