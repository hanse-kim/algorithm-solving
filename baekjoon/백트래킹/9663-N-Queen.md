# 백트래킹 - N-Queen [🔗](https://www.acmicpc.net/problem/9663)

## 문제

N-Queen 문제는 크기가 N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제이다.

N이 주어졌을 때, 퀸을 놓는 방법의 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. (1 ≤ N < 15)

## 출력

첫째 줄에 퀸 N개를 서로 공격할 수 없게 놓는 경우의 수를 출력한다.

##### 입출력 예

| 입력 | 출력 |
| ---- | ---- |
| 8    | 92   |

## 풀이

체스판을 한 행씩 순회하면서 해당 행의 각 칸 중 퀸을 놓을 수 있는 자리가 있다면 퀸을 놓고 재귀를 통해 다음 행의 순회를 수행한다. 만약 행이 체스판 범위를 벗어났다면 퀸을 한 행에 하나씩, 즉 N개 놓았다는 의미이므로 퀸을 놓은 방법의 수를 +1한다.

로직 자체는 맞지만 너무 오래 걸려서 타임아웃이 발생한다. Pypy3으로 돌리면 간신히 통과한다.

```python
def backtracking(N, queens, row=0):
    case_count = 0
    
    if row == N :
        return 1
    
    for col in range(N):
        if is_available_queen(row, col, queens):
            queens[row] = col
            case_count += backtracking(N, queens, row + 1)
    
    return case_count

def is_available_queen(row, col, queens):
    for i in range(row):
        if queens[i] == col:
            return False
        if queens[i] == col + (i - row):
            return False
        if queens[i] == col - (i - row):
            return False
    return True

N = int(input())
queens = [ -1 for _ in range(N) ]
print(backtracking(N, queens))
```
