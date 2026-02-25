# C언어 조건문과 반복문

## 개요
C언어의 제어문(조건문, 반복문) 정리. if문, switch-case문, while문, for문, do-while문을 다룬다.

---

## 조건문

### 단순 if문

```c
if (조건식)
{
    명령어1;
    명령어2;
}
```

**주의**: `if(조건식);` 처럼 세미콜론을 붙이면 중괄호 내용이 if문과 무관해진다.

```c
int Num = 102;

if (Num < 102)
{
    printf("Num < 102");  // 실행 안됨 (102 < 102는 거짓)
}
```

### if-else문

조건식이 참/거짓 모든 경우를 처리할 수 있다.

```c
if (조건식)
{
    명령어1;  // 조건식이 참이면 실행
}
else
{
    명령어2;  // 조건식이 거짓이면 실행
}
```

```c
int Num = 102;

if (Num < 105)
{
    printf("Num < 105");  // 출력됨
}
else
{
    printf("105 <= Num");
}
```

### if-else if-else문

```c
if (조건식1)
{
    명령어1;  // 조건식1이 참이면 실행
}
else if (조건식2)
{
    명령어2;  // 조건식1이 거짓이고, 조건식2가 참이면 실행
}
else
{
    명령어3;  // 모두 거짓이면 실행
}
```

**if-else if-else문 vs 단순 if문 연속**
- `if-else if-else`: 하나의 조건만 실행됨
- `if` 여러 개: 조건이 모두 참이면 모두 실행됨

### 중첩 if문 (Nested-if)

```c
if (조건식1)
{
    명령어1;

    if (조건식2)
    {
        명령어2;
    }
    else
    {
        명령어3;
    }
}
```

**좋은 습관**: 명령어가 한 줄이어도 중괄호는 꼭 적는다.

```c
// Good
if (x < 10)
{
    printf("x < 10");
}

// Bad
if (x < 10) printf("x < 10");
```

**조건문 팁**: 범위 비교 시 수평선처럼 작성하면 가독성이 좋다.

```c
// Best
if (10 <= x && x <= 15)

// Worst
if (x >= 10 && x >= 15)
```

### switch-case문

```c
switch (Num)
{
case 값1:
    명령어1;
    break;

case 값2:
    명령어2;
    break;

default:
    명령어3;
    break;
}
```

- switch-case문은 if-else if-else문으로 치환 가능
- 가독성을 위해 사용
- 범위를 다루는 조건은 switch-case로 표현 불가

**Intentional-Fallthrough**: 고의로 break를 생략하여 다음 case까지 실행

```c
switch (Num)
{
case 101:
case 102:
    printf("Num == 101 || Num == 102");  // 101, 102 둘 다 여기 실행
    break;
}
```

**case 내 변수 선언 시 중괄호 필요**

```c
case 값2:
    {
        int Var = 10;
        printf("%d", Var);
    }
    break;
```

---

## 반복문

반복문에서도 "천천히 읽기"가 중요하다. 정독 횟수가 누적되어야 정확한 속독이 가능해진다.

### while문

```c
while (조건식)
{
    명령어1;
}
```

```c
int i = 1;

while (i < 5)
{
    printf("%d ", i);  // 출력: 1 2 3 4
    ++i;
}
```

#### while문 예제: 누적합

```c
#include <stdio.h>

int main(void)
{
    int i = 0;
    int num;
    int sum = 0;

    while (i < 5)
    {
        scanf("%d", &num);
        sum += num;
        ++i;
    }

    printf("%d", sum);

    return 0;
}
```

#### while문 예제: 누적곱

```c
#include <stdio.h>

int main(void)
{
    int i = 0;
    int num;
    int product = 1;

    while (i < 5)
    {
        scanf("%d", &num);
        product *= num;
        ++i;
    }

    printf("%d", product);

    return 0;
}
```

#### while문 예제: 최대값

```c
#include <stdio.h>

int main(void)
{
    int i = 0;
    int num;
    int max;

    scanf("%d", &max);  // 첫 번째 값을 max로 초기화
    ++i;

    while (i < 5)
    {
        scanf("%d", &num);
        if (num > max)
        {
            max = num;
        }
        ++i;
    }

    printf("%d", max);

    return 0;
}
```

#### while문 예제: 최소값

```c
#include <stdio.h>

int main(void)
{
    int i = 0;
    int num;
    int min;

    scanf("%d", &min);  // 첫 번째 값을 min으로 초기화
    ++i;

    while (i < 5)
    {
        scanf("%d", &num);
        if (num < min)
        {
            min = num;
        }
        ++i;
    }

    printf("%d", min);

    return 0;
}
```

**참고**: 반복문의 순회 변수로는 `i`를 자주 사용한다 (iterate, index의 약자).

### for문

```c
for (초기식; 조건식; 증감식)
{
    명령어1;
}
```

```c
int i;

for (i = 1; i < 5; ++i)
{
    printf("%d ", i);  // 출력: 1 2 3 4
}
```

#### for문 예제: 누적합

```c
#include <stdio.h>

int main(void)
{
    int i;
    int num;
    int sum = 0;

    for (i = 0; i < 5; ++i)
    {
        scanf("%d", &num);
        sum += num;
    }

    printf("%d", sum);

    return 0;
}
```

#### for문 예제: 누적곱

```c
#include <stdio.h>

int main(void)
{
    int i;
    int num;
    int product = 1;

    for (i = 0; i < 5; ++i)
    {
        scanf("%d", &num);
        product *= num;
    }

    printf("%d", product);

    return 0;
}
```

#### for문 예제: 최대값

```c
#include <stdio.h>

int main(void)
{
    int i;
    int num;
    int max;

    scanf("%d", &max);

    for (i = 1; i < 5; ++i)
    {
        scanf("%d", &num);
        if (num > max)
        {
            max = num;
        }
    }

    printf("%d", max);

    return 0;
}
```

#### for문 예제: 최소값

```c
#include <stdio.h>

int main(void)
{
    int i;
    int num;
    int min;

    scanf("%d", &min);

    for (i = 1; i < 5; ++i)
    {
        scanf("%d", &num);
        if (num < min)
        {
            min = num;
        }
    }

    printf("%d", min);

    return 0;
}
```

### do-while문

일단 조건식 검사 없이 1회 실행(do) 후 조건식 검사.

```c
do
{
    명령어1;
} while (조건식);
```

```c
int i = 0, Boom = 5;

do
{
    printf("%d...\n", i);  // 0... 1... 2... 3... 4...
    ++i;
} while (i < Boom);

printf("BOOM!!!");
```

**do-while문은 언제 사용?**
- 자료구조 구현 등 특수한 경우에 유용
- 대부분은 for문, 순회 변수가 필요 없으면 while문 사용

### 무한 반복문

```c
while (1)
{
    명령어1;
}
```

```c
int i;
for (i = 1; ; ++i)  // 조건식 비워두면 참으로 간주
{
    명령어1;
}
```

**falsy 값**: C에서 0만 거짓, 0 이외의 값(-9 포함)은 모두 참.

### break

반복문이나 switch-case문에서 탈출. 해당 스코프에서만 탈출된다.

```c
while (1)
{
    while (2)
    {
        break;  // while (2) 반복문만 탈출
    }
}
```

### continue

해당 회차만 건너뛰고 다음 회차 진행.

```c
int i = 0;

while (1)
{
    if (i % 2 == 1)
    {
        ++i;
        continue;  // 홀수면 건너뜀
    }

    if (5 <= i)
    {
        break;
    }

    printf("%d ", i);  // 출력: 0 2 4
    ++i;
}
```
