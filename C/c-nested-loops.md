# C언어 이중 반복문

## 개요
이중 반복문(Nested Loop)의 구조와 활용법 정리. 별 찍기, 숫자 출력 등 패턴 문제에 필수적인 개념이다.

---

## 이중 반복문 기본 구조

순회 변수로 `i`, `j`, `k`, ... 순으로 작성한다.

```c
int i, j;

for (i = 초기값; i < 조건; ++i)
{
    for (j = 초기값; j < 조건; ++j)
    {
        명령어;
    }
}
```

---

## 이중 반복문 꿀팁

1. **줄의 개수**를 파악하고 줄 번호 매기기
2. 각 줄의 **칸 개수**를 파악하고 칸 번호 매기기
3. 각 칸의 **출력**을 적기

---

## 예제 코드

### 기본 예제: 3x3 별 출력

```c
#include <stdio.h>

int main(void)
{
    int i, j;

    for (i = 1; i <= 3; ++i)
    {
        for (j = 1; j <= 3; ++j)
        {
            printf("%c ", '*');
        }
        printf("\n");
    }

    return 0;
}
```

**출력 결과:**
```
* * *
* * *
* * *
```

### 숫자 테이블 출력

```c
#include <stdio.h>

int main(void)
{
    int i, j;
    int num = 1;

    for (i = 1; i <= 3; ++i)
    {
        for (j = 1; j <= 4; ++j)
        {
            printf("%2d ", num);
            ++num;
        }
        printf("\n");
    }

    return 0;
}
```

**출력 결과:**
```
 1  2  3  4
 5  6  7  8
 9 10 11 12
```

### 피라미드 출력 (응용)

반복문 안에 여러 개의 반복문이 들어가는 형태.

```c
#include <stdio.h>

int main(void)
{
    int i, j, k;

    for (i = 1; i <= 3; ++i)
    {
        // 공백 출력
        for (j = i; j < 3; ++j)
        {
            printf(" ");
        }

        // 별 출력
        for (k = 1; k <= i; ++k)
        {
            printf("*");
        }

        printf("\n");
    }

    return 0;
}
```

**출력 결과:**
```
  *
 **
***
```

### 구구단 출력

```c
#include <stdio.h>

int main(void)
{
    int i, j;

    for (i = 2; i <= 9; ++i)
    {
        printf("[ %d단 ]\n", i);
        for (j = 1; j <= 9; ++j)
        {
            printf("%d x %d = %2d\n", i, j, i * j);
        }
        printf("\n");
    }

    return 0;
}
```

### 역삼각형 출력

```c
#include <stdio.h>

int main(void)
{
    int i, j;

    for (i = 5; i >= 1; --i)
    {
        for (j = 1; j <= i; ++j)
        {
            printf("*");
        }
        printf("\n");
    }

    return 0;
}
```

**출력 결과:**
```
*****
****
***
**
*
```

---

## 핵심 정리

| 항목 | 설명 |
|------|------|
| 바깥 반복문 (i) | 줄(row) 제어 |
| 안쪽 반복문 (j) | 칸(column) 제어 |
| `printf("\n")` | 한 줄 출력 후 줄바꿈 |

**참고**: 패턴 문제는 출력 결과를 보고 역산하는 연습이 중요하다.
