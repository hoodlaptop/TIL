# C언어 배열

## 개요
배열(Array)의 개념과 1차원, 2차원 배열 활용법 정리. 자료구조의 가장 기초가 되는 개념이다.

---

## 배열이란?

- 사용할 메모리 크기를 **고정**해서 선언하는 자료구조
- 선언 후 크기 변경 **불가** (정적 자료구조)
- 메모리가 **연속적**으로 할당됨

### 배열의 필요성

변수를 5천만 개 선언한다면?
```c
int Vote1, Vote2, Vote3, ...; // 불가능
```

배열을 사용하면:
```c
int Vote[50000000]; // 간단!
```

---

## 1차원 배열

### 선언 및 초기화

```c
// 선언만
자료형 배열명[배열크기];

// 선언과 동시에 초기화
자료형 배열명[배열크기] = { 값0, 값1, ..., 값(배열크기-1) };

// 모든 요소를 0으로 초기화
int arr[10] = { 0, };
```

### 배열 호텔 비유

1. 사용할 객실 개수를 미리 예약 (크기 변경 불가)
2. 연속된 객실로 배정
3. **객실 번호(index)는 0번부터 시작**

### 예제: 배열 출력

```c
#include <stdio.h>

int main(void)
{
    int i;
    int Array[4] = { 1, 2, 3, 4 };

    for (i = 0; i < 4; ++i)
    {
        printf("%d ", Array[i]);
    }

    return 0;
}
```

**출력:** `1 2 3 4`

### 예제: 배열에 입력 받기

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>

int main(void)
{
    int i, Answer;
    int Array[4] = { 0, };

    for (i = 0; i < 4; ++i)
    {
        scanf("%d", &Answer);
        Array[i] = Answer;
    }

    for (i = 0; i < 4; ++i)
    {
        printf("%d ", Array[i]);
    }

    return 0;
}
```

### 예제: 배열 최대값 찾기

```c
#include <stdio.h>

int main(void)
{
    int i;
    int Array[5] = { 3, 7, 2, 9, 5 };
    int max = Array[0];

    for (i = 1; i < 5; ++i)
    {
        if (Array[i] > max)
        {
            max = Array[i];
        }
    }

    printf("Max: %d", max);

    return 0;
}
```

**출력:** `Max: 9`

### 예제: 배열 합계와 평균

```c
#include <stdio.h>

int main(void)
{
    int i;
    int Array[5] = { 10, 20, 30, 40, 50 };
    int sum = 0;

    for (i = 0; i < 5; ++i)
    {
        sum += Array[i];
    }

    printf("Sum: %d\n", sum);
    printf("Average: %d", sum / 5);

    return 0;
}
```

**출력:**
```
Sum: 150
Average: 30
```

---

## 2차원 배열

1차원 배열을 쌓아올린 형태로 이해. 실제 메모리상에는 1차원으로 나열된다.

### 선언 및 초기화

```c
// 선언만
자료형 배열명[줄개수][칸개수];

// 선언과 동시에 초기화
자료형 배열명[줄개수][칸개수] = {
    { 값00, 값01, ..., 값0(칸-1) },
    { 값10, 값11, ..., 값1(칸-1) },
    ...
};
```

### 예제: 2차원 배열 출력

```c
#include <stdio.h>

int main(void)
{
    int i, j;
    int Array2D[4][3] = {
        { 1,  2,  3 },
        { 4,  5,  6 },
        { 7,  8,  9 },
        { 10, 11, 12 }
    };

    for (i = 0; i < 4; ++i)
    {
        for (j = 0; j < 3; ++j)
        {
            printf("%2d ", Array2D[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

**출력:**
```
 1  2  3
 4  5  6
 7  8  9
10 11 12
```

### 예제: 2차원 배열에 입력 받기

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>

int main(void)
{
    int i, j, Input;
    int Array2D[4][3] = { 0, };

    for (i = 0; i < 4; ++i)
    {
        for (j = 0; j < 3; ++j)
        {
            scanf("%d", &Input);
            Array2D[i][j] = Input;
        }
    }

    for (i = 0; i < 4; ++i)
    {
        for (j = 0; j < 3; ++j)
        {
            printf("%2d ", Array2D[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

### 예제: 2차원 배열 행/열 합계

```c
#include <stdio.h>

int main(void)
{
    int i, j;
    int Array2D[3][3] = {
        { 1, 2, 3 },
        { 4, 5, 6 },
        { 7, 8, 9 }
    };

    // 각 행의 합계
    for (i = 0; i < 3; ++i)
    {
        int rowSum = 0;
        for (j = 0; j < 3; ++j)
        {
            rowSum += Array2D[i][j];
        }
        printf("Row %d Sum: %d\n", i, rowSum);
    }

    return 0;
}
```

**출력:**
```
Row 0 Sum: 6
Row 1 Sum: 15
Row 2 Sum: 24
```

---

## 핵심 정리

| 항목 | 1차원 배열 | 2차원 배열 |
|------|-----------|-----------|
| 선언 | `int arr[5]` | `int arr[3][4]` |
| 접근 | `arr[i]` | `arr[i][j]` |
| 인덱스 시작 | 0 | 0 |
| 메모리 | 연속 할당 | 연속 할당 (행 우선) |

**주의**: 배열의 index는 항상 **0부터 시작**한다!
