# A* 알고리즘 : 경로 탐색 알고리즘

## 개요
게임 개발에서 필수적인 탐색 알고리즘으로, 다익스트라 알고리즘을 확장한 버전이다. 휴리스틱 추정을 사용하여 효율적인 경로 탐색을 실현한다.

## 핵심 공식

```
f(n) = g(n) + h(n)
```

- **g(n)**: 시작점에서 현재 노드까지의 실제 비용
- **h(n)**: 현재 노드에서 목표까지의 예상 비용 (휴리스틱)
- **f(n)**: 전체 추정 비용 (가장 작은 값의 노드부터 탐색)

## 코드

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <cmath>
using namespace std;

struct Node {
    int x, y;
    int g, h;
    int f() const { return g + h; }

    bool operator>(const Node& other) const {
        return f() > other.f();
    }
};

// 맨해튼 거리 휴리스틱
int heuristic(int x1, int y1, int x2, int y2) {
    return abs(x1 - x2) + abs(y1 - y2);
}

// 4방향 이동
int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};

vector<pair<int,int>> AStar(vector<vector<int>>& grid,
                            pair<int,int> start,
                            pair<int,int> goal)
{
    int rows = grid.size();
    int cols = grid[0].size();

    priority_queue<Node, vector<Node>, greater<Node>> openList;
    vector<vector<bool>> closed(rows, vector<bool>(cols, false));
    vector<vector<pair<int,int>>> parent(rows, vector<pair<int,int>>(cols, {-1,-1}));

    Node startNode = {start.first, start.second, 0,
                      heuristic(start.first, start.second, goal.first, goal.second)};
    openList.push(startNode);

    while (!openList.empty()) {
        Node current = openList.top();
        openList.pop();

        if (current.x == goal.first && current.y == goal.second) {
            // 경로 역추적
            vector<pair<int,int>> path;
            int x = goal.first, y = goal.second;
            while (x != -1 && y != -1) {
                path.push_back({x, y});
                auto p = parent[x][y];
                x = p.first;
                y = p.second;
            }
            return path;
        }

        if (closed[current.x][current.y]) continue;
        closed[current.x][current.y] = true;

        for (int i = 0; i < 4; i++) {
            int nx = current.x + dx[i];
            int ny = current.y + dy[i];

            if (nx >= 0 && nx < rows && ny >= 0 && ny < cols
                && grid[nx][ny] == 0 && !closed[nx][ny]) {
                Node next = {nx, ny, current.g + 1,
                            heuristic(nx, ny, goal.first, goal.second)};
                parent[nx][ny] = {current.x, current.y};
                openList.push(next);
            }
        }
    }

    return {}; // 경로 없음
}
```

## 장점
- 휴리스틱을 통한 빠른 탐색
- 허용 가능한 휴리스틱 사용 시 최적 경로 보장
- 다양한 상황 적응 가능

## 단점
- 큰 탐색 공간에서 높은 메모리 사용
- 휴리스틱 선택에 따른 성능 편차
- 연속 공간 처리의 어려움

## 게임에서의 활용
- NPC 이동 경로 계산
- 타일 기반 맵에서의 최단 경로
- 전략 게임의 유닛 이동

## 참고
- [원본 블로그](https://hood-laptop.tistory.com/6)
