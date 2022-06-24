---
title: "[Sort] 정렬 알고리즘(nlogn)"
date: 2022-06-22 01:40:00 +09:00
categories: [Algorithms]
tags: [sort, heap, merge, quick]
mermaid: true
use_math: true
---

### 목차

- [사전지식](#사전지식)
- [O(nlogn) 정렬](#onlogn-정렬)

### 사전지식

- [시간복잡도](#시간복잡도)
- [Big-O 표기법](#big-o-표기법)
- [안정 정렬 / 불안정 정렬](#안정정렬과-불안정정렬)
- [참조 지역성 원리](#참조지역성-원리)

#### 시간복잡도

알고리즘의 시간 복잡도란 '입력값의 변화에 따라 연산을 실행할 때, 연산 횟수에 비해 시간이 얼마나 소요되는가?'를 의미합니다.  
효율적인 알고리즘이란 입력값이 커짐에 따라 증가하는 시간의 비율을 최소화한 알고리즘이라고 할 수 있습니다.  
시간 복잡도는 주로 Big-O 표기법을 사용해 나타냅니다.

#### Big-O 표기법

Big-O(빅오) 표기법을 사용하면 프로그램이 실행되는 과정에서 소요되는 가장 최악의 시간까지 고려할 수 있습니다.

따라서 평균과 가까운 성능으로 예측하기 용이하므로 Big-O 표기법을 가장 많이 사용합니다.
![BigO](/assets/img/2022-06-22/BigO.png)
_여러 함수의 증가율 비교_

#### 안정정렬과 불안정정렬

안정 정렬과 불안정 정렬의 구분은 입력에 중복된 값이 있을 경우 어떻게 정렬하는가에 따라 나뉩니다.

- 안정 정렬: 중복된 값이 있을 경우 입력 순서와 동일하게 정렬됨
  - Ex) 삽입 정렬, 버블 정렬, 병합 정렬
- 불안정 정렬: 중복된 값이 있을 경우 입력 순서와 관계없이 무작위로 정렬됨
  - Ex) 선택 정렬, 퀵 정렬, 힙 정렬

#### 참조지역성 원리

$O(nlogn)$ 시간 복잡도 알고리즘의 실제 동작 시간은 $C × nlogn + α$로 나타낼 수 있는데, 이때 $C$에 따라 같은 $nlogn$이더라도 시간 차이가 발생합니다.

이 $C$에 영향을 미치는 요소로 '알고리즘이 참조 지역성(Locality of Reference) 원리를 얼마나 잘 만족하는가'가 있습니다.

참조 지역성 원리란, CPU가 미래에 원하는 데이터를 예측하여 속도가 빠른 장치인 캐시 메모리에 담아놓는데, 이때의 예측률을 높이기 위해 사용하는 원리입니다. 쉽게 말하자면, 최근에 참조한 메모리나 그 메모리와 인접한 메모리를 다시 참조할 확률이 높다는 이론을 기반으로 캐시 메모리에 담아놓는 것입니다.

메모리를 연속으로 읽는 작업은 캐시 메모리에서 읽어오기에 빠른 반면, 무작위로 읽는 작업은 메인 메모리에서 읽어오기에 속도의 차이가 있습니다.

### O(nlogn) 정렬

- [힙 정렬](#힙-정렬heap-sort)
- [병합 정렬](#병합-정렬merge-sort)
- [퀵 정렬](#퀵-정렬quick-sort)

#### 힙 정렬(Heap Sort)

##### 힙 정렬 개념

- 자료구조 '힙(heap)'
  - 완전 이진트리의 일종으로 우선순위 큐를 위해 만들어진 자료구조
  - 최댓값, 최솟값을 쉽게 추출할 수 있는 자료구조
    ![heap](/assets/img/2022-06-22/heap.png)
  - 힙 정렬이란 최대 힙 트리나 최소 힙 트리를 구성해 정렬하는 방법을 말함

##### 힙 정렬 과정(오름차순)

1. 원소를 입력받고 최대 힙 구조로 만든다.
2. 힙에서 최댓값(루트 노드)을 꺼내 힙 배열의 끝 부분에 저장한다.
3. 힙의 최대값(루트 노드)를 가장 마지막 원소의 위치와 바꿔준다.
4. 힙의 크기를 1만큼 줄이고, 트리를 다시 최대 힙 구조로 복구한다.
5. 힙의 크기가 0이 될 때까지 2~4번 과정을 반복한다.

##### 힙 정렬 장점

- 배열을 정렬하는 데 추가 메모리를 요구하지 않음
- 최악의 경우에도 $nlogn$의 시간 복잡도

##### 힙 정렬 단점

- 불안정 정렬

##### 힙 정렬 Java Code

```java
import java.util.Arrays;
import java.util.Random;

public class HeapSort {

    static final int N = 10;

    public static void main(String[] args) {
        Random random = new Random(); // 랜덤함수를 이용

        int[] arr = new int[N];
        for (int i = 0; i < N; i++) {
            arr[i] = random.nextInt(100); // 0 ~ 99
        }

        System.out.println("정렬 전: " + Arrays.toString(arr));
        heapSort(arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }

    private static void heapSort(int[] arr) {
        int n = arr.length;

        // maxHeap을 구성
        // n/2-1 : 부모노드의 인덱스를 기준으로 왼쪽(i*2+1) 오른쪽(i*2+2)
        // 즉 자식노드를 갖는 노트의 최대 개수부터
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i); // 일반 배열을 힙으로 구성
        }

        for (int i = n - 1; i > 0; i--) {
            swap(arr, 0, i);
            heapify(arr, i, 0); // 요소를 제거한 뒤 다시 최대힙을 구성
        }
    }

    private static void heapify(int[] arr, int n, int i) {
        int p = i;
        int l = i * 2 + 1;
        int r = i * 2 + 2;

        // 왼쪽 자식노드
        if (l < n && arr[p] < arr[l])
            p = l;
        // 오른쪽 자식노드
        if (r < n && arr[p] < arr[r])
            p = r;

        // 부모노드 < 자식노드
        if (i != p) {
            swap(arr, p, i);
            heapify(arr, n, p);
        }
    }

    private static void swap(int[] arr, int a, int b) {
        int temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }
}
```

<br>

#### 병합 정렬(Merge Sort)

##### 병합 정렬 개념

병합 정렬은 각 단계에서 입력을 반으로 나눠 재귀 호출을 통해 정렬하는 방법을 말합니다.

##### 병합 정렬 과정

1. 분할(Divide): 입력 배열을 같은 크기의 2개의 배열로 분할한다.
2. 정복(Conquer): 부분 배열을 정렬한다. 부분 배열의 크기가 1 이상이면 재귀호출을 통해 다시 분할 정복을 적용한다.
3. 결합(Combine): 정렬된 부분 배열을 다시 하나의 배열로 합병하고 이 결과를 임시 배열에 저장한다.
4. 복사(Copy): 임시 배열에 저장된 결과를 원래의 배열에 복사한다.
   ![merge](/assets/img/2022-06-22/merge.png)

##### 병합 정렬 장점

- 안정 정렬
- 최악의 경우에도 $nlogn$의 시간 복잡도
- 연결 리스트 정렬에 적합함

##### 병합 정렬 단점

- 정렬되지 않은 배열과 같은 크기의 추가 공간이 필요함
- 큰 배열에 정렬에는 사용하기 어려움

##### 병합 정렬 Java Code

```java
import java.util.Arrays;
import java.util.Random;

public class MergeSort {

    static final int N = 10;

    public static void main(String[] args) {
        Random random = new Random(); // 랜덤함수를 이용

        int[] arr = new int[N];
        for (int i = 0; i < N; i++) {
            arr[i] = random.nextInt(100); // 0 ~ 99
        }

        System.out.println("정렬 전: " + Arrays.toString(arr));
        mergeSort(0, N - 1, arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }

    // divide
    private static void mergeSort(int start, int end, int[] arr) {
        if (start >= end)
            return;

        int mid = (start + end) / 2;
        mergeSort(start, mid, arr); // left
        mergeSort(mid + 1, end, arr); // right

        merge(start, mid, end, arr);
    }

    // conquer
    private static void merge(int start, int mid, int end, int[] arr) {
        int[] temp = new int[end - start + 1];
        int i = start, j = mid + 1, k = 0;

        // combine
        while (i <= mid && j <= end) {
            if (arr[i] < arr[j])
                temp[k++] = arr[i++];
            else
                temp[k++] = arr[j++];
        }
        while (i <= mid)
            temp[k++] = arr[i++];
        while (j <= end)
            temp[k++] = arr[j++];

        // copy
        while (k-- > 0)
            arr[start + k] = temp[k];
    }
}
```

<br>

#### 퀵 정렬(Quick Sort)

##### 퀵 정렬 개념

- 분할 정복과 피벗을 사용한 정렬 방법입니다.
- 퀵 정렬은 합병 정렬과 다르게 리스트를 비균등하게 분할합니다.

##### 퀵 정렬 과정

1. 리스트의 첫 번째 원소를 피벗으로 선택한다.
2. 피벗을 기준으로, 피벗보다 작은 원소들은 피벗의 왼쪽으로, 피벗보다 큰 원소들은 피벗의 오른쪽으로 옮겨진다.
3. 리스트의 크기가 0이나 1이 될 때까지 위의 반복한다.
   ![quick](/assets/img/2022-06-22/quick.png)

##### 퀵 정렬 장점

- 다른 $nlogn$ 정렬 알고리즘보다 비교적 빠름
- 추가 메모리 공간을 필요로 하지 않음

##### 퀵 정렬 단점

- 불안정 정렬
- 정렬된 리스트의 경우 불균형 분할에 의해 시간 복잡도가 $O(n2)$으로 더 많이 걸림

##### 퀵 정렬 Java Code

```java
import java.util.Arrays;
import java.util.Random;

public class QuickSort {
    static final int N = 10;

    public static void main(String[] args) {
        Random random = new Random(); // 랜덤함수를 이용

        int[] arr = new int[N];
        for (int i = 0; i < N; i++) {
            arr[i] = random.nextInt(100); // 0 ~ 99
        }

        System.out.println("정렬 전: " + Arrays.toString(arr));
        quickSort(0, N - 1, arr);
        System.out.println("정렬 후: " + Arrays.toString(arr));
    }

    private static void quickSort(int start, int end, int[] arr) {
        if (start >= end)
            return;

        int left = start + 1, right = end;
        int pivot = arr[start];

        while (left <= right) {
            while (left <= end && arr[left] <= pivot)
                left++;
            while (right > start && arr[right] >= pivot)
                right--;

            if (left <= right) {
                swap(arr, left, right);
            } else {
                swap(arr, start, right);
            }
        }
        quickSort(start, right - 1, arr);
        quickSort(right + 1, end, arr);
    }

    private static void swap(int[] arr, int a, int b) {
        int temp = arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }
}
```
