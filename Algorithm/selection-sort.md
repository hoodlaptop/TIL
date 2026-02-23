# Selection Sorting (선택 정렬)

## 개요
정렬되지 않은 배열에서 가장 작은 숫자를 찾아 정렬하는 방식이다. 버블 정렬과 유사하지만 스왑 횟수가 적어 더 효율적이다.

## 코드

```cpp
#include <iostream>
using namespace std;

#define SWAP(x, y, temp) ( (temp)=(x), (x)=(y), (y)=(temp) )

void selectionSort(int list[], int n)
{
    int minIndex, temp;

    for (int i = 0; i < n - 1; i++)
    {
        minIndex = i;
        // 최솟값 탐색
        for (int j = i + 1; j < n; j++)
        {
            if (list[j] < list[minIndex])
                minIndex = j;
        }
        // 최솟값이 자기 자신이면 이동을 하지 않는다.
        if (i != minIndex) {
            SWAP(list[i], list[minIndex], temp);
        }
    }
}

int main()
{
    const int n = 5;
    int list[n] = { 5, 12, 6, 2, 7 };

    selectionSort(list, n);

    for (int i = 0; i < n; i++)
    {
        cout << list[i] << endl;
    }

    return 0;
}
```

## 시간복잡도
- O(n²)

## 평가
버블 정렬보다 스왑 횟수가 적어 효율적이지만, 주로 사용하기에는 적합하지 않다.

## 참고
- [내 블로그](https://hood-laptop.tistory.com/3)
