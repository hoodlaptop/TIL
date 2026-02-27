# C언어 포인터

## 개요
메모리 주소를 저장하고 조작하기 위한 포인터의 개념과 사용법을 정리한다.

---

## 포인터의 필요성

5천만 개 크기의 int 배열 = 2억 바이트(약 200MB)

이 데이터를 함수에 전달할 때마다 복사하면 비효율적이다. 배열은 연속적인 메모리에 저장되므로, **자료형과 시작 주소만 알면** 모든 데이터에 접근 가능하다.

| 비유 | 크기 |
|------|------|
| 아파트 1채 | 10톤 (2억 바이트) |
| 아파트 소유 문서 | 10g (8바이트 주소값) |

---

## 포인터(Pointer) 선언

메모리 주소를 저장하기 위한 변수.

```c
자료형* 변수명 = 메모리주소값;
```

### 자료형이 필요한 이유

해당 메모리 주소에서 **얼마만큼의 크기로 읽어야** 하는지 알기 위함.
- `int*`: 4바이트씩 읽기
- `char*`: 1바이트씩 읽기
- `double*`: 8바이트씩 읽기

---

## 주소 연산자 &

피연산자의 메모리 주소를 반환하는 연산자.

```c
int X = 10;
int* PtrToX = &X;  // X의 메모리 주소를 PtrToX에 저장
```

### 메모리 주소를 얻는 두 가지 방법

| 방법 | 예시 |
|------|------|
| 주소 연산자 | `&변수명` |
| 배열 이름 | `int Array[1024];` → Array가 시작 주소 |

---

## 역참조 연산자 *

포인터가 가리키는 메모리 주소의 값을 읽거나 수정하는 연산자.

```c
int X = 10;
int* PtrToX = &X;

printf("%d\n", *PtrToX);  // 10 출력 (값 읽기)

*PtrToX = 100;            // X의 값을 100으로 수정

printf("%d\n", *PtrToX);  // 100 출력
```

---

## * 기호의 세 가지 용도

| 용도 | 위치/형태 | 예시 |
|------|----------|------|
| 포인터 선언 | 자료형 오른쪽 | `int* ptr` |
| 역참조 연산자 | 단항 연산자 | `*ptr` |
| 곱셈 연산자 | 이항 연산자 | `a * b` |

---

## 주소 출력하기

메모리 주소 출력 시 `%p` 형식 지정자 사용 (void* 형변환 필요).

```c
int Num = 100;
int* PointerToNum = &Num;

printf("&Num:          %p\n", (void*)&Num);
printf("PointerToNum:  %p\n", (void*)PointerToNum);
printf("*PointerToNum: %d\n", *PointerToNum);
```

---

## 값에 의한 호출 vs 참조에 의한 호출

### 값에 의한 호출 (Call by Value)

인자의 **복사본**이 전달되어 원본이 변경되지 않음.

```c
void Swap1(int Op1, int Op2)
{
    int Temp = Op1;
    Op1 = Op2;
    Op2 = Temp;
}  // 원본에 영향 없음
```

### 참조에 의한 호출 (Call by Reference)

인자의 **주소**를 전달하여 원본을 직접 수정.

```c
void Swap2(int* Op1, int* Op2)
{
    int Temp = *Op1;
    *Op1 = *Op2;
    *Op2 = Temp;
}  // 원본이 실제로 교환됨

// 호출
Swap2(&Num1, &Num2);
```

---

## 실전 예제: GetMinMax 함수

포인터를 활용해 함수에서 여러 값을 반환하는 패턴.

```c
void GetMinMax(const size_t InArrayLength, const int InArray[],
               int* OutPtrToMin, int* OutPtrToMax)
{
    *OutPtrToMin = InArray[0];
    *OutPtrToMax = InArray[0];

    for (size_t i = 0; i < InArrayLength; ++i)
    {
        if (InArray[i] < *OutPtrToMin)
            *OutPtrToMin = InArray[i];

        if (*OutPtrToMax < InArray[i])
            *OutPtrToMax = InArray[i];
    }
}

// 사용
int ResultMin, ResultMax;
GetMinMax(5, Nums, &ResultMin, &ResultMax);
```

---

## [심화] Right-Left Rule

복잡한 포인터 선언을 해석하는 방법.

### 규칙
1. 변수명을 기준으로 오른쪽(right)부터 읽기
2. 닫는 괄호를 만나면 반대쪽(left)으로 읽기
3. 기호 해석:
   - `*`: pointer to ~
   - `[]`: array of ~
   - `()`: function expecting ~ and returning ~

### 예시

```c
int Func(int A, int B);
// Func is function expecting (int, int) and returning int

int* (*WTF)(void);
// WTF is pointer to function expecting (void) and returning pointer to int

int* Var[3];
// Var is array of pointer to int
```

---

## 핵심 정리

| 연산자/개념 | 기호 | 역할 |
|------------|------|------|
| 포인터 선언 | `자료형*` | 메모리 주소를 저장할 변수 선언 |
| 주소 연산자 | `&` | 변수의 메모리 주소 반환 |
| 역참조 연산자 | `*` | 포인터가 가리키는 값 접근/수정 |

**주의사항**: 포인터는 강력하지만, 잘못 사용하면 다른 메모리 영역을 임의로 수정할 수 있어 위험하다.
