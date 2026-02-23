# ScriptableObject 활용법

## 개요
ScriptableObject는 Unity에서 데이터를 저장하는 데이터 컨테이너다. MonoBehaviour와 달리 씬에 붙일 필요 없이 에셋으로 저장된다.

## 장점
- 메모리 효율적 (여러 오브젝트가 같은 데이터를 참조)
- 런타임 중 값 변경이 에디터에서 저장됨
- 프리팹 의존성 줄임

## 기본 사용법

```csharp
[CreateAssetMenu(fileName = "MonsterData", menuName = "Game/Monster Data")]
public class MonsterDataSO : ScriptableObject
{
    public string monsterName;
    public int hp;
    public int attack;
    public Sprite sprite;
}
```

## 활용 예시

### 아이템 데이터
```csharp
[CreateAssetMenu(fileName = "ItemData", menuName = "Game/Item Data")]
public class ItemData : ScriptableObject
{
    public string itemName;
    public int price;
    public ItemType type;
    public Sprite icon;
}
```

### 카드 효과
```csharp
public abstract class CardEffect : ScriptableObject
{
    public abstract void Execute(CombatManager combat);
}
```

## 주의사항
- 빌드 후에는 런타임 변경값이 저장되지 않음
- 런타임 데이터는 별도 클래스로 관리 필요

## 참고
- M_IDLE_project에서 MonsterDataSO, ItemData 등에 활용
