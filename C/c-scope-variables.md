# C언어 스코프와 변수의 종류

## 개요
C언어에서 변수와 함수의 유효 범위(스코프)와 다양한 변수 종류를 정리한다.

---

## 스코프(Scope)

변수나 함수 이름을 사용할 수 있는 범위를 뜻한다.

### 스코프의 종류

| 종류 | 설명 |
|------|------|
| 블럭 스코프 | 중괄호 `{}` 내부에서만 유효 |
| 파일 스코프 | 파일 전체에서 유효 |

---

## 블럭 스코프(Block Scope)

중괄호 내부에 선언된 변수는 해당 중괄호 내부에서만 사용할 수 있다.

### 특징
- 조건문, 반복문의 중괄호 범위도 블럭 스코프
- 블럭 스코프 안에 또 다른 블럭 스코프 가능
- 바깥쪽에서 안쪽 블럭의 변수 접근 불가
- 안쪽에서 바깥쪽 블럭의 변수 접근 가능

```c
#include <stdio.h>

int main(void)
{
    int Num1 = 10;
    printf("Num1: %d\n", Num1);

    {
        int Num2 = 100;
        int Result = Num1 + Num2;  // 바깥 변수 Num1 접근 가능

        printf("Result: %d\n", Result);
        printf("Num1: %d\n", Num1);
    }

    // Num2, Result는 여기서 접근 불가
    return 0;
}
```

### 변수 가리기(Variable Shadowing) 금지

블럭 스코프가 다르면 같은 이름의 변수 선언이 가능하지만, **절대 사용하지 말 것**.

```c
int main(void)
{
    int MyScore = 87;

    {
        int MyScore = 95;  // Bad - 혼란을 야기함
        printf("MyScore: %d\n", MyScore);  // 95 출력
    }

    printf("MyScore: %d\n", MyScore);  // 87 출력
    return 0;
}
```

---

## 파일 스코프(File Scope)

어떤 블럭 스코프에도 속하지 않고 파일에 직접 작성된 경우.

```c
#include <stdio.h>

int FileScopeVariable = 0;  // 파일 스코프 변수 -> 전역변수

int ReturnZero(void)        // 파일 스코프 함수 -> 전역함수
{
    return 0;
}

int main(void)
{
    return 0;
}
```

---

## 전방 선언(Forward Declaration)

함수의 원형(머리부분)만 파일 스코프 상단에 두고, 함수의 정의는 하단에 위치시키는 방법.

```c
#include <stdio.h>

int add(int a, int b);  // 전방 선언

int main(void)
{
    printf("add(2, 5) == %d", add(2, 5));
    return 0;
}

int add(int a, int b)  // 함수 정의
{
    return a + b;
}
```

**목적**: 분할 컴파일을 위한 준비 단계

---

## 변수의 종류

### 지역 변수(Local Variable)

| 특징 | 설명 |
|------|------|
| 선언 위치 | 블럭 스코프 내부 |
| 저장 위치 | 스택 메모리 |
| 수명 | 함수 종료 시 소멸 |

```c
int add(int A, int B)
{
    int Result = A + B;  // 지역 변수
    return Result;
}  // Result는 여기서 소멸
```

### 전역 변수(Global Variable)

| 특징 | 설명 |
|------|------|
| 선언 위치 | 파일 스코프 |
| 저장 위치 | 데이터 섹션 |
| 수명 | 프로그램 전체 |

```c
int GResult;  // 전역 변수

void StoreSum(int A, int B)
{
    GResult = A + B;  // 어디서든 접근 가능
}
```

### 정적 지역 변수(Static Local Variable)

지역 변수 앞에 `static` 키워드가 붙으면 **데이터 섹션**에 저장되어 함수가 종료되어도 값이 유지된다.

```c
void MakeCoke(void)
{
    static int SCokeCount = 0;  // 정적 지역 변수 - 값 유지
    int CokeCount = 0;          // 일반 지역 변수 - 매번 초기화

    printf("SCokeCount: %d\n", SCokeCount);
    printf("CokeCount: %d\n", CokeCount);

    ++SCokeCount;  // 다음 호출 시에도 증가된 값 유지
    ++CokeCount;   // 다음 호출 시 다시 0부터 시작
}
```

**출력 결과 (3회 호출 시)**:
```
SCokeCount: 0, CokeCount: 0
SCokeCount: 1, CokeCount: 0
SCokeCount: 2, CokeCount: 0
```

### 정적 전역 변수(Static Global Variable)

전역 변수 앞에 `static`이 붙으면 **해당 파일 내에서만 접근 가능**.

```c
// Main.c
static int A;  // 이 파일에서만 접근 가능

// MyMath.c
void PrintA(void)
{
    printf("%d", A);  // 컴파일 에러! 다른 파일의 static 전역 변수
}
```

### const 변수

초기화 이후 값을 변경할 수 없는 변수. 초기화가 강제된다.

```c
const double PI = 3.141592;

PI = 3.15;  // 컴파일 에러!
```

---

## 핵심 정리

| 변수 종류 | 저장 위치 | 수명 | 접근 범위 |
|----------|----------|------|----------|
| 지역 변수 | 스택 | 함수 종료까지 | 해당 블럭 |
| 전역 변수 | 데이터 섹션 | 프로그램 전체 | 모든 파일 |
| 정적 지역 변수 | 데이터 섹션 | 프로그램 전체 | 해당 블럭 |
| 정적 전역 변수 | 데이터 섹션 | 프로그램 전체 | 해당 파일만 |

**참고**: `static` 키워드는 저장 위치를 데이터 섹션으로 변경하고, 전역 변수에 붙으면 접근 범위를 파일 내로 제한한다.
