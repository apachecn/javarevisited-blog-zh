# 解决第二最短路径问题

> 原文：<https://medium.com/javarevisited/solve-the-second-shortest-path-problem-7c1082e85049?source=collection_archive---------3----------------------->

## 通过使用 BFS 或迪杰斯特拉算法

我们知道，对于单源最短路径(SSSP)问题，我们可以

*   如果在每次探索中没有重量或重量相等或重量相等，则使用 BFS 求解。
*   如果图具有正的不相等权重，则使用 Dijkstra 求解。

对于本文中的问题，我们可以使用改进的 BFS 和 Dijkstra 来解决第二最短路径问题。

## 2045 年。到达目的地的第二最短时间

此题为 leetcode 263 周赛题第 4 题。在这篇文章中，我将介绍基于 BFS 和基于 Dijkstra 的解决方案。

# BFS 与最大访问限制

既然对于这个问题，权重是相等的，我们可以考虑用 BFS 来解决这个问题。基本步骤:

1.  使用 BFS 遍历图形。与标准 BFS 的不同之处在于，一个节点可以被访问多次。(你可以认为我们应该忽略看到的信息)
2.  如果我们首先到达目标节点，我们不应该停止(在标准的 SSSP 问题中，我们在这里停止)。我们应该做的是继续探索该图，直到为目标节点找到第二条最短路径。
3.  如果我们不对每个节点的访问次数设置限制，我们将得到 TLE，因为图的大小可以呈指数增长。我们可以使用一个未经证实的技巧，为每个节点设置一个最大访问时间的上限，这样问题就可以解决了。

参考代码可以在这里找到。

```
using PII = pair<int, int>;
int seen[10010];
class Solution {
    public:
    int secondMinimum(int n, vector<vector<int>>& edges, int time, int change) {
        memset(seen, 0, sizeof seen);
        vector<PII> thisLevel;
        int minCost = INT_MAX;
        thisLevel.emplace_back(1, 0);
        vector<vector<int>> graph(n + 1);
        for (auto e: edges){
            graph[e[0]].push_back(e[1]);
            graph[e[1]].push_back(e[0]);
        }
        seen[1]++;
        int newcost;
        const int MAX_VISIT = 70;
        // BFS with depth constraints for each node.
        while (thisLevel.size()){
            vector<PII> nextLevel;
            for (auto [node, cost]: thisLevel){
                for (auto nei : graph[node]){
                    // comment this line will get TLE.
                    if (seen[nei] > MAX_VISIT)continue;
                    seen[nei]++;
                    newcost = cost + time;
                    // red
                    if ((cost / change) % 2 == 1){
                        newcost += change - (cost % change); 
                    } 
                    if (nei == n ){
                        if (minCost!= INT_MAX && newcost > minCost){
                            return newcost;
                        } else {
                            minCost = newcost;
                        }
                    } 
                    nextLevel.emplace_back(nei, newcost);   
                }
            }thisLevel = nextLevel;
        }
        return 0;
    }
};
```

如果您对这个 BFS 解决方案不满意，我们可以转向基于 Dijkstra 的解决方案。

运行时间

```
Runtime: 1693 ms, faster than 5.50% of C++ online submissions forSecond Minimum Time to Reach Destination.Memory Usage: 362 MB, less than 5.07% of C++ online submissions forSecond Minimum Time to Reach Destination.
```

# 基于 Dijkstra 的解决方案

这个想法很简单:

*   在标准的 Dijkstra 算法中，我们为最短路径维护一个优先级队列和一个距离数组。
*   对于这个问题，我们维护两个距离数组，一个用于最短路径，一个用于第二短路径。

参考代码[1]

```
using PII = pair<int, int>;class Solution {
    public:
    int secondMinimum(int n, vector<vector<int>>& edges, int time, int change) {
        vector<int> dis(n+1, INT_MAX), dis2(n+1, INT_MAX);
        vector<vector<int>> graph(n + 1);
        for (auto &e : edges) {
            graph[e[0]].push_back(e[1]);
            graph[e[1]].push_back(e[0]);
        }
        dis[1] = 0;
        priority_queue<PII, vector<PII>, greater<PII>> pq;
        pq.emplace(0, 1);
        while (pq.size()) {
            auto [cost, node] = pq.top();pq.pop();
            if (dis2[node] < cost) continue;
            for (auto nei: graph[node]) {
                int new_cost = cost + time;
                // red
                if ((cost / change) % 2 == 1){
                    new_cost += change - (cost % change);
                }
                if (dis[nei] > new_cost){
                    dis2[nei] = dis[nei];
                    dis[nei] = new_cost;
                    pq.emplace(new_cost, nei);
                } else if (new_cost > dis[nei] && new_cost < dis2[nei] ) {
                    dis2[nei] = new_cost;
                    pq.emplace(new_cost, nei);
                }
            }
        }
        return dis2[n];
    }
};
```

运行时间

```
Runtime: 1280 ms, faster than 22.84% of C++ online submissions for Second Minimum Time to Reach Destination.Memory Usage: 181.6 MB, less than 74.34% of C++ online submissions for Second Minimum Time to Reach Destination.
```

# 优化的 BFS 解决方案

由于每次探索的权重是相等的，所以优先级队列是不必要的(常规队列已经可以使前面的元素具有最小的权重)。只需将 priority_queue 替换为 queue，就可以实现更快的解决方案。

```
using PII = pair<int, int>;
class Solution {
    public:
    int secondMinimum(int n, vector<vector<int>>& edges, int time, int change) {
        vector<int> dis(n+1, INT_MAX), dis2(n+1, INT_MAX);
        vector<vector<int>> graph(n + 1);
        for (auto &e : edges) {
            graph[e[0]].push_back(e[1]);
            graph[e[1]].push_back(e[0]);
        }
        dis[1] = 0;
        queue<PII> Q;
        Q.emplace(0, 1);
        while (Q.size()) {
            auto [cost, node] = Q.front();Q.pop();
            for (auto nei: graph[node]) {
                int new_cost = cost + time;
                // red
                if ((cost / change) % 2 == 1){
                    new_cost += change - (cost % change);
                }
                if (dis[nei] > new_cost){
                    dis2[nei] = dis[nei];
                    dis[nei] = new_cost;
                    Q.emplace(new_cost, nei);
                } else if (new_cost > dis[nei] && new_cost < dis2[nei] ) {
                    dis2[nei] = new_cost;
                    Q.emplace(new_cost, nei);
                }
            }
        }
        return dis2[n];
    }
};
```

运行时间

```
Runtime: 544 ms, faster than 92.53% of C++ online submissions forSecond Minimum Time to Reach Destination.Memory Usage: 182 MB, less than 73.56% of C++ online submissions for Second Minimum Time to Reach Destination.
```

python 版本

```
class Solution:
    def secondMinimum(self, n: int, edges: List[List[int]], time: int, change: int) -> int:
        dis, dis2 = [float("inf")]*(n+1), [float("inf")]*(n+1)
        graph = collections.defaultdict(list)
        for u, v in  edges:
            graph[u].append(v)
            graph[v].append(u)
        dis[1] = 0
        Q = deque()
        Q.append((0, 1))
        while Q:
            cost, node = Q.popleft()
            for nei in graph[node]:
                new_cost = cost + time;
                # red signal
                if (cost // change) % 2 == 1:
                    new_cost += change - (cost % change)
                # update two distances.
                if dis[nei] > new_cost:
                    dis2[nei], dis[nei]  = dis[nei], new_cost;
                    Q.append((new_cost, nei))
                elif new_cost > dis[nei] and new_cost < dis2[nei]:
                    dis2[nei] = new_cost
                    Q.append((new_cost, nei))
        return dis2[n]
```

# 参考

[1]提交自[马斯克雷](https://leetcode-cn.com/MaskRay)