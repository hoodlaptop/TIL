# 제네릭을 활용한 오브젝트 풀링

## 개요
게임에서 자주 생성/삭제되는 오브젝트(총알, 파티클, 몬스터 등)는 매번 Instantiate/Destroy하면 GC 부하가 발생한다. 오브젝트 풀링으로 재사용하면 성능이 향상된다.

## 기본 구현

```csharp
public class ObjectPool<T> where T : MonoBehaviour
{
    private Queue<T> _pool = new Queue<T>();
    private T _prefab;
    private Transform _parent;

    public ObjectPool(T prefab, int initialSize, Transform parent = null)
    {
        _prefab = prefab;
        _parent = parent;

        for (int i = 0; i < initialSize; i++)
        {
            T obj = Object.Instantiate(_prefab, _parent);
            obj.gameObject.SetActive(false);
            _pool.Enqueue(obj);
        }
    }

    public T Get()
    {
        T obj;
        if (_pool.Count > 0)
        {
            obj = _pool.Dequeue();
        }
        else
        {
            obj = Object.Instantiate(_prefab, _parent);
        }
        obj.gameObject.SetActive(true);
        return obj;
    }

    public void Return(T obj)
    {
        obj.gameObject.SetActive(false);
        _pool.Enqueue(obj);
    }
}
```

## 사용 예시

```csharp
public class ObstacleSpawner : MonoBehaviour
{
    [SerializeField] private Obstacle obstaclePrefab;
    private ObjectPool<Obstacle> _obstaclePool;

    private void Start()
    {
        _obstaclePool = new ObjectPool<Obstacle>(obstaclePrefab, 10, transform);
    }

    public void SpawnObstacle()
    {
        Obstacle obstacle = _obstaclePool.Get();
        obstacle.transform.position = spawnPosition;
        obstacle.Initialize(() => _obstaclePool.Return(obstacle));
    }
}
```

## Unity 2021+ ObjectPool

Unity에서 제공하는 기본 풀도 있다:

```csharp
using UnityEngine.Pool;

private ObjectPool<GameObject> _pool;

void Start()
{
    _pool = new ObjectPool<GameObject>(
        createFunc: () => Instantiate(prefab),
        actionOnGet: obj => obj.SetActive(true),
        actionOnRelease: obj => obj.SetActive(false),
        actionOnDestroy: obj => Destroy(obj),
        defaultCapacity: 10
    );
}
```

## 참고
- Flippy_Dwarf의 ObstacleSpawner에서 장애물 스폰에 활용 가능
