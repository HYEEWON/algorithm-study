# 최장 증가 부분 수열(LIS, Longest Increasing Subsequence)

* 배열에서 일부를 골라 만든 부분 수열 중, 모든 원소가 오름차순으로 증가하고 그 길이가 최대인 수열
* `동적 계획법`, `이분 탐색` 두 가지 방법으로 구할 수 있음
* 예) [10, 20, 10, 30, 20, 50] -> [10, 20, 30, 50]

<br>

## 동적 계획법

* 특정 원소와 해당 원소 이전의 원소들을 비교하며 DP 배열을 계산
* 시간복잡도: O(N^2) -> 이분 탐색보다 느림

```java
int [] array = new int[N]; // 원소가 N개인 배열
int [] dp = new int[N];

for (int k = 0; k < N; k++){
	dp[k] = 1;
    for (int i = 0; i < k; i++){
        if(arr[i] < array[k]){
            dp[k] = max(dp[k], dp[i] + 1);
        }        
    }
}
```

<br>

## 이분 탐색

* 특정 원소가 LIS 배열에 들어갈 수 있는 위치를 이분 탐색(`Lower Bound`)으로 찾음
* 시간 복잡도: O(NlogN) -> 동적 계획법보다 빠름

### Lower Bound 구현
* LIS의 길이와 원소를 구함

```java
public static void main(String[] args) throws IOException {  
    int[] array = new int[N]; // 원본 배열
    int[] index = new int[N]; // index[i] = j // 원본 배열의 i 인덱스의 숫자는 LIS에서 j인덱스에 존재
    index[0] = 0;

    ArrayList<Integer> answer = new ArrayList<>(); // lis 배열
    answer.add(array[0]);
          
    for (int i=1; i<N; ++i) {
        if (array[i] > answer.get(answer.size()-1))
            answer.add(array[i]);
        else {
            int idx = lowerBound(array[i]);
            answer.set(idx, array[i]);
            index[i] = idx;
        }                
    }
          
    count = answer.size(); // lis 길이
    count -= 1;
    for (int i = array.length-1; i>=0; --i) {
        if (index[i] == count) {
            answer.set(count, array[i]);
            count--; continue;
        }
    }
}
  
public static int lowerBound(int n) {
    int start = 0; int end = answer.size() - 1;
    int mid = 0;
   
    while (start < end) {
        mid = (start + end) / 2;
    
        if (n > answer.get(mid))
            start = mid + 1;
        else
            end = mid;
    }
    return end;
}
```

### 자바 내부의 binarySearch() 메서드 이용

```java
public static int lis() {
    array = new int[N];
    lisArray = new int[N];

    lisArray[0] = array[0];
    int idx = 1; // LIS의 길이
    
    for (int i = 1; i < N; i++) {
        if (lisArray[idx-1] < array[i]) {     
            lisArray[idx] = array[i];
            idx++; 
        }
        else {
            int tmpIdx = Arrays.binarySearch(lisArray, 0, idx, array[i]);
            if (tmpIdx < 0)
                tmpIdx = (tmpIdx * (-1)) - 1;
            lisArray[tmpIdx] = array[i];
        }
    }
    return idx;
} 
```