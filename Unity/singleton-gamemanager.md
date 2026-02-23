# 싱글톤 패턴으로 GameManager 만들기

## 개요
게임 전체에서 하나만 존재해야 하는 매니저 클래스를 만들 때 싱글톤 패턴을 사용한다.

## 기본 구현

```csharp
public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }

    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }
}
```

## 제네릭 싱글톤

여러 매니저에서 재사용할 수 있는 제네릭 버전:

```csharp
public class Singleton<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T _instance;

    public static T Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = FindObjectOfType<T>();
            }
            return _instance;
        }
    }

    protected virtual void Awake()
    {
        if (_instance == null)
        {
            _instance = this as T;
            DontDestroyOnLoad(gameObject);
        }
        else if (_instance != this)
        {
            Destroy(gameObject);
        }
    }
}
```

### 사용법
```csharp
public class SoundManager : Singleton<SoundManager>
{
    public void PlaySFX(string name) { ... }
}

// 호출
SoundManager.Instance.PlaySFX("attack");
```

## 주의사항
- 씬 전환 시 DontDestroyOnLoad 필요
- 너무 많은 싱글톤은 커플링 증가 -> 의존성 주입 고려

## 참고
- Flippy_Dwarf의 SoundManager, GameManager에서 활용
