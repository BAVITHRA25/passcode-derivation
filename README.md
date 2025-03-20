# passcode-derivation
from collections import defaultdict
import heapq

attempts = [input() for _ in range(int(input()))]
adj, deg = defaultdict(set), defaultdict(int)
chars = set().union(*attempts)

for attempt in attempts:
    for i, j in zip(attempt, attempt[1:]):
        if j not in adj[i]:
            adj[i].add(j)
            deg[j] += 1

heap = [c for c in chars if not deg[c]]
heapq.heapify(heap)
result = []

while heap:
    curr = heapq.heappop(heap)
    result.append(curr)
    for next_digit in adj[curr]:
        deg[next_digit] -= 1
        if not deg[next_digit]:
            heapq.heappush(heap, next_digit)

print("".join(result) if len(result) == len(chars) else "SMTH WRONG")
