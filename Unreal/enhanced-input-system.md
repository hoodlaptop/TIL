# 언리얼 Enhanced Input System

## 개요
언리얼 엔진 5의 Enhanced Input System을 활용한 캐릭터 이동, 카메라 회전, 점프 구현 정리.

---

## Enhanced Input System이란?

- UE5의 새로운 입력 시스템
- 기존 입력 시스템보다 유연하고 확장성 높음
- Input Action과 Input Mapping Context로 구성

### 핵심 구성 요소

| 요소 | 설명 |
|------|------|
| Input Action (IA) | 개별 입력 동작 정의 (Move, Jump 등) |
| Input Mapping Context (IMC) | 입력 액션들을 그룹으로 묶어 관리 |
| Enhanced Input Local Player Subsystem | 입력 컨텍스트 등록/관리 |

---

## BeginPlay: 입력 컨텍스트 등록

게임 시작 시 Input Mapping Context를 등록해야 입력이 동작한다.

### 블루프린트 구조

```
Event BeginPlay
    ↓
Get Player Controller (Player Index: 0)
    ↓
Enhanced Input Local Player Subsystem
    ↓
Add Mapping Context
    - Mapping Context: IMC_Default (또는 사용자 정의 IMC)
    - Priority: 0
```

### 주요 노드

| 노드 | 설명 |
|------|------|
| Get Player Controller | 플레이어 컨트롤러 가져오기 |
| Enhanced Input Local Player Subsystem | Enhanced Input 서브시스템 |
| Add Mapping Context | 입력 컨텍스트 등록 |

---

## Move: 캐릭터 이동

WASD 입력을 받아 캐릭터를 이동시킨다.

### 블루프린트 구조

```
EnhancedInputAction IA_Move
    ↓
Triggered
    ↓
Get Control Rotation (Target: self)
    ├─ Return Value X (Roll) → 0.0
    ├─ Return Value Y (Pitch) → 0.0
    └─ Return Value Z (Yaw) → Get Forward Vector / Get Right Vector
                                        ↓
                              Add Movement Input
                                - Target: self
                                - World Direction: Forward/Right Vector
                                - Scale Value: Action Value X/Y
```

### 상세 설명

1. **EnhancedInputAction IA_Move**: 이동 입력 이벤트
   - `Action Value X`: 좌우 입력 (A/D)
   - `Action Value Y`: 전후 입력 (W/S)

2. **Get Control Rotation**: 카메라(컨트롤러) 회전값 가져오기
   - Yaw 값만 사용하여 이동 방향 계산

3. **Get Forward Vector / Get Right Vector**: 회전값에서 방향 벡터 추출

4. **Add Movement Input**: 실제 이동 적용
   - Scale Value로 이동 속도 조절

### 코드 버전 (C++)

```cpp
void AMyCharacter::Move(const FInputActionValue& Value)
{
    FVector2D MovementVector = Value.Get<FVector2D>();

    if (Controller != nullptr)
    {
        const FRotator Rotation = Controller->GetControlRotation();
        const FRotator YawRotation(0, Rotation.Yaw, 0);

        const FVector ForwardDirection = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::X);
        const FVector RightDirection = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::Y);

        AddMovementInput(ForwardDirection, MovementVector.Y);
        AddMovementInput(RightDirection, MovementVector.X);
    }
}
```

---

## Look: 카메라 회전

마우스 입력을 받아 카메라를 회전시킨다.

### 블루프린트 구조

```
EnhancedInputAction IA_Look
    ↓
Triggered
    ├─ Action Value X → Add Controller Yaw Input
    │                     - Target: self
    │                     - Val: Action Value X
    │
    └─ Action Value Y → Add Controller Pitch Input
                          - Target: self
                          - Val: Action Value Y
```

### 상세 설명

| 입력 | 함수 | 효과 |
|------|------|------|
| 마우스 X | Add Controller Yaw Input | 좌우 회전 |
| 마우스 Y | Add Controller Pitch Input | 상하 회전 |

### 코드 버전 (C++)

```cpp
void AMyCharacter::Look(const FInputActionValue& Value)
{
    FVector2D LookAxisVector = Value.Get<FVector2D>();

    if (Controller != nullptr)
    {
        AddControllerYawInput(LookAxisVector.X);
        AddControllerPitchInput(LookAxisVector.Y);
    }
}
```

---

## Jump: 점프

스페이스바 입력으로 점프를 실행한다.

### 블루프린트 구조

```
EnhancedInputAction IA_Jump
    ↓
Triggered
    ↓
Jump (Target is Character)
    - Target: self
```

### 상세 설명

- Character 클래스의 내장 Jump 함수 사용
- `Character Movement` 컴포넌트의 설정에 따라 점프 높이 결정

### 코드 버전 (C++)

```cpp
void AMyCharacter::StartJump(const FInputActionValue& Value)
{
    if (Value.Get<bool>())
    {
        Jump();
    }
}

void AMyCharacter::StopJump(const FInputActionValue& Value)
{
    StopJumping();
}
```

---

## Input Action 설정

### IA_Move
- Value Type: `Axis2D (Vector2D)`
- Triggers: None (매 프레임 체크)

### IA_Look
- Value Type: `Axis2D (Vector2D)`
- Triggers: None

### IA_Jump
- Value Type: `Digital (bool)`
- Triggers: Pressed

---

## Input Mapping Context 설정

IMC에서 키 바인딩 예시:

| Action | Key | Modifiers |
|--------|-----|-----------|
| IA_Move | W | Swizzle Input Axis (YXZ) |
| IA_Move | S | Swizzle Input Axis (YXZ), Negate |
| IA_Move | A | Negate |
| IA_Move | D | None |
| IA_Look | Mouse XY 2D-Axis | None |
| IA_Jump | Space Bar | None |

---

## 핵심 정리

| 기능 | Input Action | 주요 노드 |
|------|--------------|----------|
| 이동 | IA_Move | Get Control Rotation, Add Movement Input |
| 카메라 | IA_Look | Add Controller Yaw/Pitch Input |
| 점프 | IA_Jump | Jump |

**참고**: Legacy Input 대신 Enhanced Input 사용을 권장 (UE5 기본값)
