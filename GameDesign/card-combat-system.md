# 카드 기반 전투 시스템 설계

## 개요
Slay the Spire 스타일의 덱빌딩 카드 전투 시스템 설계 방법.

## 핵심 구조

### 1. 카드 데이터 (ScriptableObject)
```csharp
[CreateAssetMenu(fileName = "Card", menuName = "Game/Card")]
public class Card : ScriptableObject
{
    public string cardName;
    public string description;
    public int cost;
    public Sprite artwork;
    public CardEffect effect;
}
```

### 2. 카드 효과 (Strategy 패턴)
```csharp
public abstract class CardEffect : ScriptableObject
{
    public abstract void Execute(PlayerCombatStatus player, MonsterStatus target);
}

// 구체적인 효과들
public class BasicAttack : CardEffect
{
    public int damage;

    public override void Execute(PlayerCombatStatus player, MonsterStatus target)
    {
        target.TakeDamage(damage + player.bonusDamage);
    }
}

public class DefensiveStance : CardEffect
{
    public int armor;

    public override void Execute(PlayerCombatStatus player, MonsterStatus target)
    {
        player.AddArmor(armor);
    }
}
```

### 3. 전투 매니저
```csharp
public class CombatManager : MonoBehaviour
{
    private List<Card> _deck = new List<Card>();
    private List<Card> _hand = new List<Card>();
    private List<Card> _discardPile = new List<Card>();

    public void DrawCards(int count)
    {
        for (int i = 0; i < count; i++)
        {
            if (_deck.Count == 0) ShuffleDiscardIntoDeck();
            if (_deck.Count == 0) return;

            Card card = _deck[0];
            _deck.RemoveAt(0);
            _hand.Add(card);
        }
    }

    public void PlayCard(Card card, MonsterStatus target)
    {
        if (currentEnergy < card.cost) return;

        currentEnergy -= card.cost;
        card.effect.Execute(player, target);
        _hand.Remove(card);
        _discardPile.Add(card);
    }
}
```

## 확장 가능한 효과들

| 효과 | 설명 |
|------|------|
| BasicAttack | 기본 공격 |
| PiercingStrike | 방어력 무시 공격 |
| PoisonAttack | 독 부여 |
| HealingPrayer | 체력 회복 |
| DefensiveStance | 방어력 증가 |
| PowerCharge | 다음 공격력 증가 |

## 장점
- 새 카드 추가가 쉬움 (새 CardEffect 만들면 됨)
- 밸런스 조절이 편함 (ScriptableObject 수정)
- 카드 조합에 따른 전략성

## 참고
- M_IDLE_project의 CardEffects 폴더 참고
