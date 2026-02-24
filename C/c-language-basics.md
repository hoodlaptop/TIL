# C언어 기초

## 개요
C언어 프로그래밍의 기초 개념 정리. 코딩의 정의부터 자료형까지 핵심 내용을 다룬다.

## 코딩이란?
코딩이란 **컴퓨터가 이해 가능한 명령서를 작성하는 과정**이다.
- "우리"가 이해 가능한 것이 아닌, "컴퓨터"가 이해 가능한 명령서
- 컴퓨터는 유추 능력이 없으므로 섬세하게 작성해야 함

## 프로그래밍 언어의 종류

### 기계어 (Low-Level Language)
- 컴퓨터(기계)가 바로 이해할 수 있는 2진법 언어
- 반도체는 켜진 상태(1)와 꺼진 상태(0)로 모든 것을 표현

### 어셈블리어 (Assembly Language)
- 기계어의 특정 부분을 문자로 치환한 언어
- 기계어와 1:1 대응 관계

### 고급 언어 (High-Level Language)
- C, C++, Java, Python 등
- 사람에 가깝다는 의미에서 High-Level이라 부름

## 빌드 프로세스

```
소스코드 → 전처리 → 컴파일 → 어셈블 → 링킹 → 실행파일
```

- **컴파일**: 소스코드를 어셈블리/오브젝트 코드로 변환하는 과정
- **빌드**: 전체 과정 (컴파일 + 링킹)

## main() 함수

프로그램의 시작점(Entry Point). 모든 C 프로그램에 반드시 필요하다.

```c
int main(void)
{
    return 0;
}
```

## printf() 함수

print formatted의 약자. 콘솔 화면에 출력하는 함수.

```c
#include <stdio.h>

int main(void)
{
    printf("Hello, world!");
    return 0;
}
```

- `stdio.h`: 표준 입출력 헤더 파일

## 탈출 문자열 (Escape Sequence)

| 문자 | 출력 |
|------|------|
| `\n` | 개행 (New line) |
| `\t` | 탭 (Tab) |
| `\'` | 따옴표 출력 |
| `\"` | 쌍따옴표 출력 |
| `\\` | 역슬래시 출력 |
| `%%` | % 출력 |

## 서식 지정자 (Format Specifier)

| 지정자 | 설명 |
|--------|------|
| `%d` | 10진수 정수 (decimal) |
| `%o` | 8진수 (octal) |
| `%x` | 16진수 소문자 (hexadecimal) |
| `%X` | 16진수 대문자 |
| `%u` | 부호 없는 정수 (unsigned) |
| `%c` | 문자 (character) |
| `%s` | 문자열 (string) |
| `%f` | 실수 (float) |
| `%lf` | 배정밀도 실수 (double) |

### 서식 지정자 활용 예시

```c
printf("[%7d]\n", 65536);   // 우측 정렬, 7자리
printf("[%-7d]\n", 65536);  // 좌측 정렬, 7자리
printf("[%07d]\n", 65536);  // 0으로 채우기
printf("[%.2f]\n", 3.14);   // 소수점 2자리
```

## 리터럴 (Literal)

소스코드에 적힌 값 그 자체.

```c
65536;           // int 리터럴
65536LL;         // long long 리터럴
65536u;          // unsigned 리터럴
3.141592;        // double 리터럴
3.141592f;       // float 리터럴
'd';             // char 리터럴
"Hello, world!"; // 문자열 리터럴
```

## 자료형 (Type)

저장될 데이터의 크기와 해석 방법에 대한 정보.

| 자료형 | 크기 | 범위 | 서식 지정자 |
|--------|------|------|-------------|
| `char` | 1 byte | -128 ~ 127 | `%c`, `%hhd` |
| `short` | 2 byte | -32,768 ~ 32,767 | `%hd` |
| `int` | 4 byte | -2,147,483,648 ~ 2,147,483,647 | `%d` |
| `long long` | 8 byte | -(2^63) ~ (2^63)-1 | `%lld` |
| `float` | 4 byte | 유효 6~7자리 | `%f` |
| `double` | 8 byte | 유효 15~16자리 | `%lf` |

## 좋은 습관

1. **시작이 있으면 끝을 맺기**: 열었다면 무조건 닫기
2. **들여쓰기 생활화**: 처음부터 습관화하기

## Visual Studio 단축키

- `F5`: 빌드 및 실행
- `Ctrl + B`: 빌드만 수행
- `Ctrl + Shift + A`: 새 소스파일 추가

## 변수 (Variable)

변할 수 있는 수. 선언 형식: `자료형 변수명 = 값;`

```c
int Num = 2147483647;  // 선언과 동시에 초기화
double PI;             // 선언만
PI = 3.141592;         // 이후 초기화
```

### unsigned와 signed

- `unsigned`: 부호 없음 (양수만), 양수 범위 2배
- `signed`: 부호 있음 (기본값, 생략 가능)

```c
unsigned char UnsignedCharNum = 255;  // 0 ~ 255
signed char SignedCharNum = -128;     // -128 ~ 127
```

### 오버플로우 (Overflow)

자료형이 표현 가능한 범위를 넘어서는 경우. 그릇에 물이 넘치는 것과 같다.

```c
unsigned char num = 255;
num = num + 1;  // 오버플로우 → 0이 됨
```

## ASCII

### 컴퓨터는 문자를 이해할 수 없다

문자는 인코딩 과정을 거쳐 숫자로 변환되어 저장된다.

- **인코딩(Encoding)**: 형태 A → 형태 B 변환
- **디코딩(Decoding)**: 형태 B → 형태 A 변환

### ASCII (American Standard Code for Information Interchange)

문자와 숫자 사이의 대표적인 인코딩 규약.

```c
char ch = 'A';
printf("%c\n", ch);      // A 출력
printf("%d\n", ch);      // 65 출력 (ASCII 코드)
printf("%c\n", ch + 2);  // C 출력
```

### 유니코드 (Unicode)

전세계 모든 문자를 표현하기 위한 문자 인코딩. UTF-8, UTF-16, UTF-32가 있다.

## scanf() 함수

키보드로부터 데이터를 입력받는 함수.

```c
#define _CRT_SECURE_NO_WARNINGS  // stdio.h 인클루드 전에 정의

#include <stdio.h>

int main(void)
{
    int Num;
    scanf("%d", &Num);  // &: 주소 연산자
    printf("%d", Num);
    return 0;
}
```

## 참고
- Visual Studio Community 설치: https://visualstudio.microsoft.com/ko/vs/community/
