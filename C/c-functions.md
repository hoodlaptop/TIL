# C언어 함수와 스택 프레임

## 개요
함수(Function)의 개념, 작성법, 그리고 스택 프레임(Stack Frame)의 동작 원리 정리.

---

## 함수란?

- 프로그래밍 언어에서 **코드 뭉치**
- 반복되는 코드를 함수로 만들어 **재사용성** 향상
- `printf()`, `scanf()`, `main()` 모두 함수

### 함수의 필요성

`printf()` 함수가 없다면 매번 내부 코드를 작성해야 한다:
```c
// printf() 없이 출력하려면 수십 줄의 코드가 필요...
```

함수를 사용하면:
```c
printf("Hello World!");  // 한 줄로 끝!
```

---

## 함수 작성법 (암기 필수!)

```c
반환자료형 함수명(매개변수자료형 매개변수명)  // 함수 선언 및 정의
{
    // 함수 본문

    return 반환값;
}

int main(void)
{
    함수명(인자값);  // 함수 호출

    return 0;
}
```

---

## 함수 예제

### 예제 1: 별 하나 출력 함수

```c
#include <stdio.h>

void PrintOneStar(void)
{
    printf("*");
}

int main(void)
{
    PrintOneStar();
    PrintOneStar();
    PrintOneStar();

    return 0;
}
```

**출력:** `***`

### 예제 2: 두 수의 합 출력 함수

```c
#include <stdio.h>

void Add(int a, int b)
{
    printf("%d + %d = %d\n", a, b, a + b);
}

int main(void)
{
    Add(3, 5);
    Add(10, 20);

    return 0;
}
```

**출력:**
```
3 + 5 = 8
10 + 20 = 30
```

### 예제 3: 두 수의 합 반환 함수

```c
#include <stdio.h>

int Add(int a, int b)
{
    return a + b;
}

int main(void)
{
    int result;

    result = Add(3, 5);
    printf("Result: %d\n", result);

    result = Add(100, 200);
    printf("Result: %d\n", result);

    return 0;
}
```

**출력:**
```
Result: 8
Result: 300
```

### 예제 4: 최대값 반환 함수

```c
#include <stdio.h>

int GetMax(int a, int b)
{
    if (a > b)
    {
        return a;
    }
    else
    {
        return b;
    }
}

int main(void)
{
    int max;

    max = GetMax(10, 25);
    printf("Max: %d\n", max);

    max = GetMax(100, 50);
    printf("Max: %d\n", max);

    return 0;
}
```

**출력:**
```
Max: 25
Max: 100
```

### 예제 5: 팩토리얼 함수

```c
#include <stdio.h>

int Factorial(int n)
{
    int i;
    int result = 1;

    for (i = 1; i <= n; ++i)
    {
        result *= i;
    }

    return result;
}

int main(void)
{
    printf("5! = %d\n", Factorial(5));
    printf("7! = %d\n", Factorial(7));

    return 0;
}
```

**출력:**
```
5! = 120
7! = 5040
```

---

## 스택 프레임 (심화)

### 함수 호출 비용

- 게임의 핵심 함수(Tick 등)는 1초에 수천 번 호출될 수 있음
- 60 FPS = 특정 함수가 1초에 60번 호출
- 따라서 **함수 호출 비용**은 최대한 적어야 함

### 스택(Stack) 자료구조

- **LIFO** (Last In, First Out): 나중에 들어간 것이 먼저 나옴
- 탄알집, 수건함과 비슷한 개념
- 함수 호출을 빠르고 저렴하게 처리

### 메모리 레이아웃

```
+------------------+
|   Stack Memory   |  ← 함수 호출 시 사용 (지역변수)
+------------------+
|   Heap Memory    |  ← 동적 할당
+------------------+
|   Data Section   |  ← 전역변수, static
+------------------+
|   Code Section   |  ← 실행 코드
+------------------+
```

### 스택 프레임이란?

- 함수가 호출되면 해당 함수가 사용할 메모리 공간 확보
- 이 확보된 공간을 **스택 프레임**이라고 함
- 함수 종료 시 스택 프레임 반환(해제)

### 스택 포인터와 베이스 포인터

| 포인터 | 설명 |
|--------|------|
| ESP (Stack Pointer) | 현재 스택의 Top 위치 |
| EBP (Base Pointer) | 현재 스택 프레임의 시작 주소 |

### 스택 프레임 동작 예시

```c
#include <stdio.h>

int add(int a, int b)
{
    return a + b;
}

int main(void)
{
    int res;

    res = add(2, 5);

    return 0;
}
```

**어셈블리 수준 동작:**

```nasm
_main:
    pushl %ebp           # 이전 베이스 포인터 저장
    movl %esp, %ebp      # 새 베이스 포인터 설정
    subl $16, %esp       # 지역변수 공간 확보
    ...
    call _add            # add() 함수 호출
    ...
    addl $16, %esp       # 스택 정리
    popl %ebp            # 베이스 포인터 복원
    ret

_add:
    pushl %ebp           # 이전 베이스 포인터 저장
    movl %esp, %ebp      # 새 베이스 포인터 설정
    subl $36, %esp       # 지역변수 공간 확보
    ...
    addl $36, %esp       # 스택 정리
    popl %ebp            # 베이스 포인터 복원
    ret
```

**스택 프레임 변화:**

```
1. main() 호출
   +------------------+
   | main() 스택프레임 |
   +------------------+

2. add() 호출
   +------------------+
   | add() 스택프레임  |  ← ESP
   +------------------+
   | main() 스택프레임 |
   +------------------+

3. add() 종료 후
   +------------------+
   | main() 스택프레임 |  ← ESP
   +------------------+
```

---

## 핵심 정리

| 용어 | 설명 |
|------|------|
| 반환자료형 | 함수가 반환하는 값의 자료형 |
| 매개변수 | 함수가 전달받는 값 |
| 인자 | 함수 호출 시 전달하는 실제 값 |
| return | 값을 반환하고 함수 종료 |
| 스택 프레임 | 함수 호출 시 확보되는 메모리 공간 |

**참고**: 스택 프레임 개념은 포인터, 재귀 함수 등 고급 개념의 기초가 된다.
