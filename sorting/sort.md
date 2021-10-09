# 정렬

<br>

## 병합 정렬

* 최선, 평균, 최악: O(NlogN)
* `안정 정렬`, `분할 정복`
* 과정
  * 배열을 반으로 쪼개 가면서 하나의 원소를 가진 배열로 만듦 
  * 쪼개진 각 배열을 정렬하면서 병합
  * 최종 정렬된 배열을 완성

```java
static int[] array = new int[N]; // 원본 배열
static int[] sorted = new int[N]; // 합치는 과정에서 정렬된 원소를 저장하는 임시 배열

// 호출
mergeSort(0, array.length-1);

public static void mergeSort(int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort(left, mid);
        mergeSort(mid+1, right);
        merge(left, mid, right);
    }
}

public static void merge(int left, int mid, int right) {
     int l = left, k = left, m = mid+1;

    // 작은 순서대로 배열에 추가
    while (l <= mid && m <= right) {
        if (array[l] <= array[m]) sorted[k++] = array[l++];
        else sorted[k++] = array[m++];
    }

    // 남은 데이터도 추가
    if (l > mid) {
        for (int i=m; i<=right; ++i)
            sorted[k++] = array[i];
    }
    else {
        for (int i=l; i<=mid; ++i)
            sorted[k++] = array[i];
    }

    // 정렬된 배열을 원본 배열에 삽입
    for (int i=left; i<=right; ++i)
        array[i] = sorted[i];
}
```

<br>

## 퀵 정렬

* 최선, 평균: O(NlogN) / 최악: O(N^2)
  * 최악인 경우: 정렬된 배열에서 피봇을 최대/최소 값으로 선택
* `불안정 정렬`,  `분할 정복`
* `파티셔닝`을 재귀적으로 활용
  * `파티셔닝`: pivot 원소를 기준으로 왼쪽은 pivot보다 작은 원소들로 모으고 오른쪽은 pivot보다 큰 원소로 모으는 것
  * pivot을 기준으로 파티셔닝이 완료되면 pivot을 고정하고 같은 과정을 `재귀호출` 하여 반복하며 정렬
* 과정 (오름차순)
  * 일반적으로 첫 번째 데이터를 피봇으로 설정
  * 피봇을 제외, 왼쪽부터 피봇보다 큰 값을, 오른쪽부터 피봇보다 작은 값을 탐색
  * 왼쪽 값 > 오른쪽 값 -> 두 값을 교환
  * 두 인덱스 교차 -> 피봇과 두 값 중 작은 값을 교환 (-> 피봇의 왼쪽은 피봇보다 작고 오른쪽은 피봇보다 커짐)
  * 피봇을 고정하고 같은 과정을 반복(재귀)

```java
quickSort(0, array.length-1);

public static void quickSort(int low, int high) {
    // 원소가 1개인 경우
    if (low >= high) return;

    int mid = partition(low, high);
    quickSort(low, mid-1);
    quickSort(mid+1, high);
}

public static int partition(int low, int high) {
    int pivot = array[low];
    int i = low+1, j = high;

    while (i <= j) {
        // 키 값보다 큰 값을 찾음
        while (i <= high && array[i] <= pivot) i++;

        // 키 값보다 작은 값을 찾음
        while (j > low && array[j] >= pivot) j--;

        // 교차되었다면 피봇과 작은 값을 교환
        if (i > j) {
            int temp = array[j];
            array[j] = pivot;
            array[low] = temp;
        }
        else {
            int temp = array[i];
            array[i] = array[j];
            array[j] = temp;
        }
    }
    return j;
}
```

<br>

## 선택 정렬

* 최선, 평균, 최악: O(N^2) 
* `불안정 정렬`
* 과정
  * 가장 작은 값을 가지는 데이터를 찾음
  * 가장 작은 값을 앞에서부터 채워 나가면서 정렬하는 방식
  * 1번의 순환마다 맨 앞의 값이 고정

```java
public static void selectionSort() {
    for (int i=0; i<array.length-1; ++i) {
        for (int j=i+1; j<array.length; ++j) {
            if (array[i] > array[j]) {
                int temp = array[i];
                array[i] = array[j];
                array[j] = temp;
            }
        }
    }
}
```

<br>

## 버블 정렬

* 최선, 평균, 최악: O(N^2)
* `안정 정렬`
* 과정
  * 처음부터 끝까지 현재 자신과 자신의 다음 데이터를 비교 및 교환
  * 큰 값을 맨 뒤로 보내는 방식으로 동작
  * 1번의 순환마다 맨 뒤의 값이 고정

```java
public static void bubbleSort() {
    for (int i=array.length-1; i>0; ++i) {
        for (int j=0; j<i; ++j) {
            if(array[j] > array[j+1]) {
                int tmp = array[j];
                array[j] = array[j+1];
                array[j+1] = tmp;
            }
        }
    }
}
```

<br>

## 삽입 정렬

* 최선: O(N) / 평균, 최악: O(N^2)
* 과정
  * 2번째 원소부터 시작하여 그 앞의 원소들과 비교하여 삽입할 위치를 지정
  * 원소를 뒤로 옮기고 지정된 자리에 자료를 삽입하여 정렬

```java
public static void insertionSort() {
    int j;
    for (int i = 1; i < array.length; ++i) {
        int key = array[i];
        for (j=i-1; j>=0 && array[j]>key; --j)
            array[j+1] = array[j];
        array[j+1] = key;
    }
}
```

<br>

## 힙 정렬
* 최선, 평균, 최악: O(NlogN) 
* `불안정 정렬`
* 완전 이진 트리를 기본으로 하는 힙에 데이터를 삽입하고 꺼내서 힙을 통해 정렬