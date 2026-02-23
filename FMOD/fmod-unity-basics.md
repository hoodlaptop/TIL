# Unity에서 FMOD 기본 사용법

## 개요
FMOD는 게임용 오디오 미들웨어로, Unity의 기본 오디오 시스템보다 강력한 기능을 제공한다.

## 장점
- 사운드 디자이너와 협업 용이
- 파라미터 기반 동적 사운드
- 3D 공간 오디오
- 메모리 효율적인 스트리밍

## 설치
1. FMOD Unity Integration 다운로드
2. Unity에 패키지 임포트
3. FMOD Studio 프로젝트 연결

## 기본 사용법

### EventReference로 사운드 재생
```csharp
using FMODUnity;
using FMOD.Studio;

public class SoundManager : MonoBehaviour
{
    [field: SerializeField]
    public EventReference AttackSound { get; private set; }

    public void PlayAttack()
    {
        RuntimeManager.PlayOneShot(AttackSound);
    }
}
```

### 위치 기반 3D 사운드
```csharp
public void PlaySoundAtPosition(EventReference sound, Vector3 position)
{
    RuntimeManager.PlayOneShot(sound, position);
}
```

### 인스턴스로 제어
```csharp
private EventInstance _bgmInstance;

public void PlayBGM(EventReference bgm)
{
    _bgmInstance = RuntimeManager.CreateInstance(bgm);
    _bgmInstance.start();
}

public void StopBGM()
{
    _bgmInstance.stop(FMOD.Studio.STOP_MODE.ALLOWFADEOUT);
    _bgmInstance.release();
}
```

### 파라미터 조절
```csharp
// FMOD Studio에서 설정한 파라미터 조절
_bgmInstance.setParameterByName("Intensity", 0.8f);
```

## StudioEventEmitter 컴포넌트

코드 없이 Inspector에서 설정 가능:
1. 오브젝트에 StudioEventEmitter 컴포넌트 추가
2. Event 필드에 FMOD 이벤트 할당
3. Play On Awake, Stop On Disable 등 설정

## 참고
- Flippy_Dwarf에서 TitleSoundManager, SoundManager로 활용
