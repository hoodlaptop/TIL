# 언리얼 Animation State Machine

## 개요
언리얼 엔진의 Animation Blueprint에서 State Machine을 활용한 캐릭터 애니메이션 전환 시스템 정리.

---

## State Machine이란?

- 캐릭터의 **상태(State)**를 정의하고 상태 간 **전환(Transition)**을 관리
- 각 상태는 특정 애니메이션과 연결
- 조건에 따라 자동으로 상태 전환

### 기본 상태 구조

```
Entry → Idle ↔ Run
          ↓
     Idle/Run → Jump → Fall → Land → Idle
```

---

## 상태(State) 정의

### 기본 상태들

| 상태 | 설명 | 애니메이션 |
|------|------|-----------|
| Idle | 대기 상태 | Idle Animation |
| Run | 이동 중 | Run/Walk Animation |
| Idle/Run | Idle과 Run을 블렌딩 | Blend Space |
| Jump | 점프 시작 | Jump Start Animation |
| Fall | 공중 낙하 | Fall Loop Animation |
| Land | 착지 | Land Animation |

### Idle/Run 블렌드 스페이스

속도에 따라 Idle과 Run 애니메이션을 자연스럽게 블렌딩:

```
Speed = 0     → Idle Animation
Speed > 0    → Walk/Run Animation (속도에 비례)
```

---

## 전환(Transition) 조건

### Idle ↔ Run

```
Idle → Run: Speed > 0
Run → Idle: Speed == 0
```

### Idle/Run → Jump

```
조건: Is Jumping == true (또는 Is In Air)
```

### Jump → Fall

```
조건: 점프 애니메이션 종료 (Time Remaining <= 0.1)
```

### Fall → Land

```
조건: Is In Air == false (땅에 닿음)
```

### Land → Idle

```
조건: 착지 애니메이션 종료 (Time Remaining <= 0.1)
```

---

## Animation Blueprint 변수

### 필수 변수

| 변수명 | 타입 | 설명 |
|--------|------|------|
| Speed | Float | 캐릭터 이동 속도 |
| IsInAir | Boolean | 공중에 있는지 여부 |
| IsJumping | Boolean | 점프 중인지 여부 |

### Event Graph에서 변수 업데이트

```
Event Blueprint Update Animation
    ↓
Try Get Pawn Owner
    ↓
Get Velocity → Vector Length → Set Speed
    ↓
Get Movement Component → Is Falling → Set IsInAir
```

---

## State Machine 생성 방법

### 1. Animation Blueprint 생성

```
Content Browser → 우클릭 → Animation → Animation Blueprint
→ Parent Class: AnimInstance
→ Skeleton: 캐릭터 스켈레톤 선택
```

### 2. AnimGraph에서 State Machine 추가

```
AnimGraph → 우클릭 → Add New State Machine
→ 이름: Locomotion (또는 원하는 이름)
```

### 3. State 추가

```
State Machine 더블클릭 → 우클릭 → Add State
→ 각 상태에 애니메이션 연결
```

### 4. Transition 설정

```
State → 드래그하여 다른 State로 연결
→ Transition Rule 더블클릭
→ 조건 노드 추가
```

---

## 전환 규칙 블루프린트 예시

### Idle → Run

```
Get Speed
    ↓
> (Greater Than) 0.0
    ↓
Result (Boolean)
```

### Jump → Fall

```
Time Remaining (Ratio)
    ↓
<= (Less Than or Equal) 0.1
    ↓
Result (Boolean)
```

### Fall → Land

```
NOT Is In Air
    ↓
Result (Boolean)
```

---

## Blend Space 활용

### 1D Blend Space

속도에 따른 애니메이션 블렌딩:

```
Axis: Speed (0 ~ 600)

0     : Idle
150   : Walk
600   : Run
```

### 2D Blend Space

방향과 속도에 따른 블렌딩:

```
Horizontal Axis: Direction (-180 ~ 180)
Vertical Axis: Speed (0 ~ 600)

각 방향/속도 조합에 애니메이션 배치
```

---

## 실전 예제: 전체 State Machine

```
                    ┌─────────────────────────────────────┐
                    │                                     │
                    ▼                                     │
Entry ──→ [Idle] ←────→ [Run]                            │
              │                                           │
              │ (Is Jumping)                              │
              ▼                                           │
          [Jump]                                          │
              │                                           │
              │ (Animation End)                           │
              ▼                                           │
          [Fall]                                          │
              │                                           │
              │ (NOT Is In Air)                           │
              ▼                                           │
          [Land] ─────────────────────────────────────────┘
              (Animation End)
```

---

## 디버깅 팁

### State Machine 실시간 확인

1. Play In Editor 시작
2. Animation Blueprint 열기
3. 상단 Debug Filter에서 캐릭터 선택
4. 현재 활성 상태가 하이라이트됨

### 일반적인 문제 해결

| 문제 | 원인 | 해결 |
|------|------|------|
| 상태 전환 안됨 | 전환 조건 미충족 | 변수 값 확인 |
| 애니메이션 끊김 | Transition 시간 없음 | Blend Time 추가 |
| T-Pose 출력 | 애니메이션 미연결 | State에 애니메이션 확인 |

---

## 핵심 정리

| 요소 | 역할 |
|------|------|
| State | 특정 애니메이션 재생 |
| Transition | 상태 간 전환 조건 정의 |
| Blend Space | 파라미터 기반 애니메이션 블렌딩 |
| Event Graph | 변수 업데이트 (Speed, IsInAir 등) |

**참고**: State Machine은 복잡한 애니메이션 로직을 시각적으로 관리할 수 있게 해주는 강력한 도구이다.
