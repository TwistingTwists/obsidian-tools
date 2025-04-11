python code 

```python
def canCross(stones):
    stone_set = set(stones)
    last_stone = stones[-1]
    dp = {stone: set() for stone in stones}
    dp[0].add(0)  # at position 0, we had a jump of 0

    for stone in stones:
        for k in dp[stone]:
            for step in [k - 1, k, k + 1]:
                if step > 0 and stone + step in stone_set:
                    dp[stone + step].add(step)

    return len(dp[last_stone]) > 0

```


space optimised 

```python
def canCross(stones):
    stone_set = set(stones)
    last_stone = stones[-1]
    visited = set()
    from collections import deque

    queue = deque()
    queue.append((0, 0))  # position, last_jump

    while queue:
        pos, jump = queue.popleft()

        for next_jump in [jump - 1, jump, jump + 1]:
            if next_jump <= 0:
                continue
            next_pos = pos + next_jump
            if next_pos == last_stone:
                return True
            if next_pos in stone_set and (next_pos, next_jump) not in visited:
                visited.add((next_pos, next_jump))
                queue.append((next_pos, next_jump))

    return False

```