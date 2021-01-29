# 탐욕법 - 섬 연결하기[🔗](https://programmers.co.kr/learn/courses/30/lessons/42861)

## 풀이

주어진 `costs`를 비용 기준으로 오름차순 정렬한다.

`connections` 리스트로 각 섬의 연결 상태를 저장하는데, 리스트 요소로 집합이 들어간다. 각 집합은 연결된 섬들을 의미한다. (만약 `[{a, b}, {c, d, e}]`라면, a섬과 b섬이 연결되어 있고 c, d, e 섬이 연결되어 있으며,  두 집합 간에는 연결이 없다)

`costs`를 순회하며 다리가 연결되는 두 섬 a, b에 대해 다음을 수행한다.

- `connections`에 존재하는 연결 중, a와 b가 각각 어느 연결에 포함되는지 그 인덱스를 구한다.
- a와 b가 동일한 연결에 포함되어 있을 경우, 이들은 이미 연결된 섬이므로 무시한다.
- a와 b 중 어느 하나만 연결에 포함되어 있는 경우, 나머지 하나를 그 연결에 추가한다.
- a와 b가 각각 다른 연결에 포함되어 있는 경우, 두 연결을 합친다.
- 무시한 경우를 제외하고 `total_cost` 값을 계산한다.

최종적으로 계산된 `total_cost`를 반환한다.

```python
def solution(n, costs):
    costs.sort(key=lambda x: x[2])
    total_cost = 0
    connections = []
    
    for a, b, cost in costs:
        a_belongs = b_belongs = -1
        for i, c in enumerate(connections):
            if a in c:
                a_belongs = i
            if b in c:
                b_belongs = i
            if a_belongs != -1 and b_belongs != -1:
                break
        
        if a_belongs != -1 and a_belongs == b_belongs:
            continue
            
        if a_belongs == -1 and b_belongs == -1:
            connections.append({a, b})
        elif a_belongs != -1 and b_belongs == -1:
            connections[a_belongs].add(b)
        elif b_belongs != -1 and a_belongs == -1:
            connections[b_belongs].add(a)
        else:
            connections[a_belongs] = connections[a_belongs] | connections[b_belongs]
            connections.pop(b_belongs)
        total_cost += cost
    
    return total_cost
```

## 풀이 - Kruskal 알고리즘 [🔗](https://www.youtube.com/watch?v=LQ3JHknGy8c)

가장 적은 비용으로 모든 노드를 연결하기 위해 사용하는 알고리즘. ( = 최소 비용 신장 트리를 만드는 알고리즘)

* 간선들을 비용을 기준으로 오름차순 정렬한다.
* 간선을 하나씩 그래프에 포함시키는데 이때 사이클 테이블을 확인한다.
* 사이클을 형성하는 경우 간선을 포함시키지 않는다.

사이클 확인에 [Union-Find 알고리즘](https://www.youtube.com/watch?v=AMByrd53PHM)이 사용된다는 점을 제외하면 원리상 위 알고리즘과 큰 차이는 없어 보인다.

Kruskal보다는 Union-Find를 확실하게 짚고 넘어가야 할 것 같다.

```python
def get_head(node, table):
    if table[node] == node:
        return node
    return get_head(table[node], table)

def solution(n, costs):
    costs.sort(key=lambda x: x[2])
    table = [ i for i in range(n) ]
    total_cost = 0
    bridge_count = 0
    for a, b, cost in costs:
        a_head = get_head(a, table)
        b_head = get_head(b, table)
        if a_head == b_head:
            continue
        table[max(a_head, b_head)] = min(a_head, b_head)
        total_cost += cost
        bridge_count  += 1
        if bridge_count == n - 1:
            break
    
    return total_cost
```

