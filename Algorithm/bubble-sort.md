# Bubble Sorting (버블 정렬)

## 개요
배열의 2개의 값을 선택하고, 비교하는 정렬이다. 왼쪽 값이 오른쪽보다 크면 두 값을 교환하며, 반복을 통해 작거나 큰 값을 끝으로 이동시킨다.

## 코드

```cpp
#include <iostream>
using namespace std;

void bubble_sort(int list[], int n) {
    int temp;
    for(int i = n - 1; i > 0; i--) {
        for(int j = 0; j < i; j++) {
            if(list[j] < list[j + 1]) {
                temp = list[j];
                list[j] = list[j + 1];
                list[j + 1] = temp;
            }
        }
    }
}

int main() {
    int n = 5;
    int list[n] = {5, 12, 6, 2, 7};
    bubble_sort(list, n);
    for(int i = 0; i < n; i++)
        cout << list[i] << endl;
    return 0;
}
```

## 시간복잡도
- 최악의 경우: O(n²)
- 최선의 경우: O(n²)

## 평가
최악의 경우 배열의 모든 부분을 반복하기에 효율적인 정렬법이 아니다.

## 참고
- [내 블로그](https://hood-laptop.tistory.com/2)
