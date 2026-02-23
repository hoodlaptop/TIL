# Insertion Sorting (삽입 정렬)

## 개요
점차적으로 정렬의 범위를 늘려가는 정렬이다. 새로운 범위의 마지막 원소를 앞으로 이동시키면서 더 작은 요소를 찾을 때까지 위치를 교체한다.

## 정렬 과정 (오름차순)
1. 2번째 원소부터 비교 시작
2. 이전 위치 원소들과 대상 원소 비교
3. 대상이 더 작으면 위치 변경
4. 이전 데이터도 차례대로 비교
5. 과정 반복

## 코드

```cpp
#include <iostream>
using namespace std;

void insertionSort(int list[], int n)
{
    int key, j;

    for (int i = 1; i < n; i++)
    {
        key = list[i];
        j = i - 1;

        // key보다 큰 원소들을 오른쪽으로 이동
        while (j >= 0 && list[j] > key)
        {
            list[j + 1] = list[j];
            j--;
        }
        list[j + 1] = key;
    }
}

int main()
{
    const int n = 5;
    int list[n] = { 5, 12, 6, 2, 7 };

    insertionSort(list, n);

    for (int i = 0; i < n; i++)
    {
        cout << list[i] << endl;
    }

    return 0;
}
```

## 시간복잡도
- 최악의 경우: O(n²)
- 최선의 경우: O(n) - 이미 정렬된 경우

## 평가
선택 정렬과 버블 정렬에 비해 구현이 간단하고 빠르고 안정적이다. 하지만 배열이 길어질수록 효율이 저하되고 임시변수가 필요하다는 단점이 있다.

## 참고
- [원본 블로그](https://hood-laptop.tistory.com/4)
