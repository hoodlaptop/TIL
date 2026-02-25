# 언리얼 블루프린트 기초

## 개요
언리얼 엔진 블루프린트를 활용한 게임 로직 구현 정리. 조건문(Branch), 변수, 입력 이벤트, 배열을 다룬다.

---

## 핵심 노드 정리

### 입력 이벤트 노드

| 노드 | 설명 |
|------|------|
| `Left Mouse Button` | 좌클릭 입력 (Pressed/Released) |
| `Keyboard Events (R, T, 1, 2, 3...)` | 키보드 입력 |

### 흐름 제어 노드

| 노드 | 설명 |
|------|------|
| `Branch` | 조건문 (True/False 분기) |
| `Sequence` | 순차 실행 (Then 0, Then 1, ...) |

### 변수 노드

| 노드 | 설명 |
|------|------|
| `SET` | 변수 값 설정 |
| `GET` | 변수 값 가져오기 |

### 출력 노드

| 노드 | 설명 |
|------|------|
| `Print String` | 화면에 문자열 출력 (디버그용) |

---

## 과제 1: 기본 총알 발사 시스템

### 변수
- `Bullet` (Integer): 현재 총알 수

### 로직 흐름

#### 발사 (좌클릭)
```
Left Mouse Button (Pressed)
    ↓
Branch (Bullet > 0)
    ├─ True  → Print "Bang!" (빨간색) → SET Bullet = Bullet - 1
    └─ False → Print "Out of bullets!"
```

#### 재장전 (R키)
```
R Key (Pressed)
    ↓
Branch (Bullet < 30)
    ├─ True  → Print "Reloaded!" (초록색) → SET Bullet = 30
    └─ False → Print "Already full"
```

### 핵심 개념
- **Branch 노드**: C언어의 if-else문과 동일
- **비교 연산**: `>`, `<` 노드로 조건 생성
- **Print String**: Text Color로 출력 색상 변경 가능

---

## 과제 2: 총알 + 온도 시스템

### 변수
- `Bullet` (Integer): 현재 총알 수
- `Current Temperature` (Integer): 현재 온도
- `Current Heat State` (String): 현재 열 상태

### 온도 상태 분류

| 상태 | 조건 |
|------|------|
| Normal | Temperature < 30 |
| High | 30 <= Temperature < 70 |
| Over | Temperature >= 70 |

### 로직 흐름

#### 발사 (좌클릭)
```
Left Mouse Button (Pressed)
    ↓
Branch (Bullet > 0)
    ├─ True  → Print "Bang!"
    │           ↓
    │         Sequence
    │           ├─ Then 0: SET Bullet = Bullet - 1
    │           └─ Then 1: SET Current Temperature = Current Temperature + 1
    │                        ↓
    │                      온도 상태 체크 (중첩 Branch)
    │
    └─ False → Print "Out of bullets!"
```

#### 온도 상태 체크 (중첩 Branch)
```
Branch (Current Temperature < 30)
    ├─ True  → SET Current Heat State = "Normal" → Print String
    └─ False → Branch (Current Temperature < 70)
                   ├─ True  → SET Current Heat State = "High" → Print String
                   └─ False → SET Current Heat State = "Over" → Print String
```

#### 냉각 (T키)
```
T Key (Pressed)
    ↓
SET Current Temperature = Current Temperature - 1
    ↓
온도 상태 체크 (위와 동일)
```

#### 재장전 (R키)
```
R Key (Pressed)
    ↓
Branch (Bullet < 30)
    ├─ True  → Print "Reloaded!" → SET Bullet = 30
    └─ False → Print "Already full"
```

### 핵심 개념
- **Sequence 노드**: 여러 작업을 순차적으로 실행
- **중첩 Branch**: if-else if-else문 구현
- **상태 관리**: 문자열 변수로 상태 저장

---

## 과제 3: 무기 교체 시스템 (배열)

### 변수
- `Current Weapon Index` (Integer): 현재 무기 인덱스
- `New Weapons` (Array): 무기 정보 배열

### 무기 배열 구조

| Index | Weapon Name | Bullet | Temperature | Heat State |
|-------|-------------|--------|-------------|------------|
| 0 | Rifle | 30 | 0 | Normal |
| 1 | ShotGun | 30 | 0 | Normal |
| 2 | Pistol | 30 | 0 | Normal |

### 로직 흐름

#### 무기 교체 (숫자키)
```
1 Key (Pressed) → SET Current Weapon Index = 0 → Print "Equip Rifle!"
2 Key (Pressed) → SET Current Weapon Index = 1 → Print "Equip ShotGun!"
3 Key (Pressed) → SET Current Weapon Index = 2 → Print "Equip Pistol!"
```

#### 발사 시 배열 업데이트
```
Left Mouse Button (Pressed)
    ↓
Branch (Bullet > 0)
    ├─ True  → Print "Bang!"
    │           ↓
    │         Sequence
    │           ├─ Then 0: 총알/온도 업데이트
    │           └─ Then 1: Set Array Elem (현재 무기 데이터 저장)
    │                        - Target Array: New Weapons
    │                        - Index: Current Weapon Index
    │                        - Item: (Bullet, Temperature, Heat State, Weapon Name)
    │
    └─ False → Print "Out of bullets!"
```

#### 무기 교체 시 배열에서 데이터 로드
```
무기 교체 이벤트
    ↓
Set Array Elem (이전 무기 데이터 저장)
    ↓
GET Array Element (새 무기 데이터 로드)
    ↓
각 변수에 로드된 값 SET
```

### 핵심 개념
- **Array (배열)**: 여러 데이터를 인덱스로 관리
- **Set Array Elem**: 배열의 특정 인덱스에 값 저장
- **GET Array Element**: 배열에서 특정 인덱스의 값 가져오기
- **구조체 활용**: Bullet, Temperature, Heat State, Weapon Name을 하나의 구조로 묶어 관리

---

## 블루프린트 팁

### 좋은 습관
1. **변수명은 명확하게**: `Bullet`, `CurrentTemperature` 등
2. **Print String 활용**: 디버그 시 색상으로 구분
3. **Sequence 노드**: 순차 실행이 필요할 때 사용

### C언어와 비교

| C언어 | 블루프린트 |
|-------|-----------|
| `if-else` | Branch |
| `for/while` | ForLoop, WhileLoop |
| `int a = 10;` | SET 노드 |
| `printf()` | Print String |
| `배열[i]` | Get/Set Array Elem |

### 자주 쓰는 단축키
- `B + 클릭`: Branch 노드 생성
- `S + 클릭`: Sequence 노드 생성
- `Ctrl + W`: 노드 복제
