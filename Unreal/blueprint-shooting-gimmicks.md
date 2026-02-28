# 블루프린트 사격 시스템 및 기믹 구현

## 개요
레이캐스트 기반 사격 시스템, 나이아가라 이펙트, 카메라 줌, 리스폰 로직 등 다양한 블루프린트 기능 구현

---

## 1. 사격 시스템 및 이펙트

### 총구 화염 및 사운드

**문제**: 이동 중 사격 시 총구 화염이 캐릭터를 따라가지 못함

**해결**: `Spawn System at Location` 대신 `Spawn System Attached`를 사용하여 소켓에 부착

![총구 화염 소켓 부착](images/muzzle-flash-sound.png)

| 설정 | 값 | 설명 |
|------|-----|------|
| System Template | NS_MuzzleFlash | 나이아가라 파티클 |
| Attach to Component | Rifle | 무기 메시 컴포넌트 |
| Attach Point Name | Muzzle | 소켓 이름 |
| Auto Destroy | True | 자동 파괴 (Loop 문제 해결) |
| Auto Activate | True | 자동 활성화 |

**나이아가라 Loop 문제**: `Auto Destroy`를 체크하면 파티클이 끝난 후 자동으로 파괴되어 무한 반복 문제 해결

---

### 레이캐스트 피격 처리

카메라 기준 전방으로 Line Trace를 발사하여 피격 지점에 이펙트 스폰

![레이캐스트 피격 이펙트](images/line-trace-hit-effect.png)

**구현 흐름**:
1. `Get World Location` + `Get Forward Vector`로 카메라 방향 계산
2. Forward Vector에 거리(5000) 곱하여 End 지점 설정
3. `Line Trace By Channel` (Visibility 채널) 실행
4. Hit 성공 시 `Break Hit Result`로 Location, Impact Normal 추출
5. `Make Rot from X`로 Normal 방향 회전 계산
6. `Spawn System at Location`으로 피격 이펙트 스폰

---

### 파괴 가능한 오브젝트

레이캐스트에 맞으면 데미지를 받고 폭발하는 타겟 액터 구현

**1단계: 데미지 전달 (사격 블루프린트)**

![Apply Damage 노드](images/destructible-object-damage.png)

- `Apply Damage` 노드로 Hit Actor에 데미지 전달
- Base Damage: 10.0

**2단계: 데미지 수신 (타겟 액터)**

![Event AnyDamage 처리](images/destructible-object-event.png)

```
Event AnyDamage
    → Play Sound at Location (폭발음)
    → Spawn System at Location (폭발 이펙트, Scale 10.0)
    → Destroy Actor
```

**3단계: 콜리전 설정**

![콜리전 프리셋](images/collision-preset-settings.png)

| 설정 | 값 |
|------|-----|
| Collision Presets | BlockAllDynamic |
| Collision Enabled | Collision Enabled (Query and Physics) |
| Object Type | WorldDynamic |
| Visibility/Camera | Block |

---

## 2. 카메라 줌 시스템

### 타임라인 기반 부드러운 줌인/줌아웃

우클릭으로 3인칭 카메라의 FOV를 부드럽게 변경하고 십자선 UI 표시

![줌 타임라인 구현](images/zoom-timeline-fov.png)

**구현 흐름**:
1. `Right Mouse Button` Pressed → Crosshair UI Visible + Timeline Play
2. `Right Mouse Button` Released → Crosshair UI Hidden + Timeline Reverse
3. Timeline Update → `Lerp`(A: 90, B: 45, Alpha) → `Set Field Of View`

| 상태 | FOV | UI |
|------|-----|-----|
| 기본 | 90 | 숨김 |
| 줌인 | 45 | 표시 |

**Accessed None 오류 주의**: Crosshair UI 변수가 유효한지 확인 필요 (Create Widget 후 변수 저장)

---

## 3. 월드 리스폰 시스템

### 맵 밖 낙하 감지 및 재스폰

Z축 좌표가 특정 값 이하로 떨어지면 안전한 위치로 재스폰

![낙하 리스폰 로직](images/fall-respawn-logic.png)

**구현 흐름**:
1. `Event Tick`에서 매 프레임 체크
2. `Get World Location` (Mesh) → Z값 추출
3. Z < -200 이면 Branch True
4. `Set Actor Location`으로 Respawn Location 이동

| 노드 | 역할 |
|------|------|
| Get World Location | 현재 위치의 Z값 확인 |
| Branch | -200 미만인지 비교 |
| Set Actor Location | 미리 지정한 Respawn Location으로 이동 |

**Respawn Location**: Vector 변수로 미리 정의된 안전한 스폰 지점

---

## 핵심 정리

| 기능 | 핵심 노드 | 주요 설정 |
|------|-----------|-----------|
| 총구 이펙트 | Spawn System Attached | Auto Destroy = True |
| 레이캐스트 | Line Trace By Channel | Visibility 채널 |
| 데미지 시스템 | Apply Damage / Event AnyDamage | Base Damage 설정 |
| 줌 | Timeline + Lerp + Set FOV | FOV 90→45 |
| 리스폰 | Event Tick + Branch | Z < -200 체크 |

**참고**:
- 나이아가라 파티클 무한 반복 시 Auto Destroy 체크
- UI 참조 오류 시 변수 유효성 확인 필요
- 콜리전은 Visibility 채널 Block 필수
