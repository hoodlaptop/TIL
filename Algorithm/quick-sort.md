# Quick Sorting (퀵 정렬)

## 개요
특정한 값(피벗)을 기준으로 큰 숫자와 작은 숫자를 나누는 정렬이다. 분할 정복(Divide and Conquer) 방식을 사용한다.

## 알고리즘 3단계

1. **분할(Divide)**: 피벗을 기준으로 배열을 두 부분으로 나눔 (왼쪽: 피벗보다 작은 요소, 오른쪽: 큰 요소)
2. **정복(Conquer)**: 부분 배열을 재귀적으로 정렬
3. **결합(Combine)**: 정렬된 부분 배열들을 합병

## 코드

```cpp
#include <iostream>
using namespace std;

void QuickSort(int *list, int start, int end)
{
    if (start >= end) return;

    int pivot = start;
    int i = pivot + 1;
    int j = end;

    while (i <= j) {
        while (i <= end && list[i] <= list[pivot]) i++;
        while (j > start && list[j] >= list[pivot]) j--;

        if (i > j) {
            swap(list[j], list[pivot]);
        } else {
            swap(list[i], list[j]);
        }
    }

    QuickSort(list, start, j - 1);
    QuickSort(list, j + 1, end);
}

int main()
{
    const int n = 5;
    int list[n] = { 5, 12, 6, 2, 7 };

    QuickSort(list, 0, n - 1);

    for (int i = 0; i < n; i++)
    {
        cout << list[i] << endl;
    }

    return 0;
}
```

## 시간복잡도
- 평균: O(n log n)
- 최악의 경우: O(n²) - 이미 정렬된 경우

## 장점
- 다른 정렬 알고리즘 대비 가장 빠름
- 대규모 데이터에서도 효율적

## 단점
- 이미 정렬된 리스트에서는 불균형 분할로 인해 성능 저하 발생

## 참고
- [원본 블로그](https://hood-laptop.tistory.com/5)
