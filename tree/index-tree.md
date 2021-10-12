# 인덱스 트리 (Index Tree)

## 1) 정의
* `이진 트리`의 응용
* 리프 노드들은 배열의 값 자체를 가지고 있음
* `구간 합`, `구간의 최대/최소값`을 구할 때 사용
* `포화 이진 트리` (세그먼트 트리는 포화가 아니어도 됨)

## 2) 구현
* 세그먼트 트리의 구현보다 속도 빠름

```java
public static void main(String[] args) throws IOException {
    int[] array = new int[N]; // 원본 배열 (N개)

    int offset = 1 << (int)(Math.ceil(Math.log(N) / Math.log(2)));
    long[] tree = new long[offset*2]; // 인덱스 트리

    // 리라벨링
    int[] relabel = Arrays.stream(array).distinct().sorted().toArray();
}

public static void init() {
    for (int i=0; i<N; ++i) 
        tree[i+offset] = array[i];
    
    for (int i=offset-1; i>=1; --i)
        tree[i] = tree[i<<1] + tree[i<<1 | 1];
}

public static void update(int idx, int val) {
    idx += offset;
    tree[idx] = val;

    idx >>= 1;
    while (idx>=1) {
        tree[idx] = tree[idx<<1] + tree[idx<<1 | 1];
        idx >>= 1;
    }
}

public static long query(int left, int right) {
    long result = 0;
    left += offset;
    right += offset;

    while (left < right) {
        if ((left&1) > 0)
            result += tree[left++];
        if ((right&1) == 0)
            result += tree[right--];
        left >>= 1;
        right >>= 1;
    }
    if (left == right)
        result += tree[left];
    return result;
}

public static int findIndex(int kth) {
    int idx = 1;

    while (idx < offset) {
        idx <<= 1;
        if (tree[idx] < kth) {
            kth -= tree[idx];
            idx++;
        }
    }
    return idx - offset;
}
```