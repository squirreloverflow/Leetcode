最短路径问题：

Dijkstra 算法：  

Dijkstra 算法是一种求解单源最短路径问题的贪婪算法，适用于带权有向图和无向图。算法的基本思想是为每个顶点 v 保留一个距离值 dist[v]，表示从源顶点 s 到顶点 v 的最短距离的上界。算法的关键思想是每次找到当前距离值最小的未访问顶点，然后根据它更新相邻顶点的距离值。

步骤：

1.初始化  
2.创建一个集合 S，用于存储已访问的节点。  
3.创建一个距离数组 dist，其中 dist[v] 表示从源节点 s 到节点 v 的最短距离。  
4.将源节点 s 的距离值设为 0（dist[s] = 0），其他所有节点的距离值设为无穷大（dist[v] = ∞, v ≠ s）。保证所有未被访问过的节点在第一次被访问时都可以被更新。  
5.创建一个优先队列（或最小堆）Q，用于存储节点及其距离值，并将所有节点添加到 Q 中。  
6.从优先队列 Q 中选出具有最小距离值的节点 u。这保证了在每一步中，我们总是从当前未访问节点中选择距离源节点最近的那个。  
7.检查节点 u 是否为目标节点。如果是，则算法结束，我们找到了最短路径。否则，继续执行下一步。  
8.更新邻接节点的距离值：遍历节点 u 的所有邻接节点 v，计算通过节点 u 到达节点 v 的距离（即 dist[u] + weight(u, v)）。如果这个距离小于当前节点 v 的距离值 dist[v]，则更新节点 v 的距离值（dist[v] = dist[u] + weight(u, v)）。这意味着我们找到了一条更短的路径到达节点 v。  
9.标记节点 u 为已访问，将其从优先队列 Q 中移除。这意味着我们已经找到了从源节点 s 到节点 u 的最短路径，不需要再次访问节点 u。  
10.重复步骤 2-5，直到优先队列 Q 为空或找到目标节点。当优先队列为空时，表示我们已经访问了所有可达节点，并找到了从源节点 s 到所有可达节点的最短路径。  
结果：最短路径的距离值存储在 dist 数组中，可以通过 dist 数组找到从源节点 s 到任何其他节点 v 的最短距离。

如果需要重建最短路径：

1.从目标节点开始，初始化一个空列表（或栈）用于存储路径上的节点。  
2.检查当前节点的前驱节点。如果存在前驱节点，将前驱节点添加到列表（或压入栈）中，然后将当前节点设置为前驱节点。重复此步骤，直到到达源节点。  
3.如果使用列表，将其反转以获得正确的节点顺序。如果使用栈，将栈中的元素依次弹出以获得正确的节点顺序。可以在更新距离值时记录前驱节点。例如，可以创建一个前驱数组 prev，将节点 v 的前驱节点设置为节点 u（prev[v] = u），然后从目标节点开始逆向遍历，直到到达源节点。 

Dijkstra算法总是找寻在现有更新状态下，距离源节点最近的节点，可能有些节点并没有被更新完（依旧是无穷大），如果所有的权都为正，因为每次更新选出的节点都是最短节点，要到达无穷大节点，在它之前的节点已经不是最短节点了，所以最后得到的结果一定是最小节点。  
如果有负权，这可能没被更新的过的节点会小于已经更新的最小节点，可能会出现错误。  
负权重环则最短路径问题没意义，总可以无限多次的绕环以得到更小的路径距离。  
如何判断负环待更新。  

LeetCode 743. Network Delay Time

题目描述：有 N 个网络节点，从 1 到 N 标记。给定一个列表 times，表示信号从节点 u 传播到节点 v 所需的时间 w。现在，从某个节点 K 发出一个信号。需要找出多久后，所有节点都收到了信号。如果不是所有节点都能收到信号，则返回 -1。

解题思路：这是一个单源最短路径问题，可以使用 Dijkstra 算法求解。从节点 K 开始，遍历所有邻接节点，更新它们的距离值。然后，选择距离值最小的未访问节点，重复此过程。最后，返回最大的距离值

c++:
```c++
#include <vector>
#include <queue>
#include <limits>
#include <unordered_map>
using namespace std;

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        // 构建邻接表表示的图
        unordered_map<int, vector<pair<int, int>>> graph;
        for (const auto& edge : times) {
            graph[edge[0]].emplace_back(edge[1], edge[2]);
        }

        // 使用优先队列作为最小堆存储距离值和节点
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.emplace(0, K);

        // 用来存储源节点到其他节点的最短距离
        unordered_map<int, int> dist;
        while (!pq.empty()) {
            int time = pq.top().first;
            int node = pq.top().second;
            pq.pop();

            // 如果节点尚未访问，将其添加到 dist 中
            if (dist.find(node) == dist.end()) {
                dist[node] = time;
                // 遍历邻接节点，将它们的距离值添加到优先队列中
                for (const auto& neighbor : graph[node]) {
                    int next_node = neighbor.first;
                    int edge_time = neighbor.second;
                    if (dist.find(next_node) == dist.end()) {
                        pq.emplace(time + edge_time, next_node);
                    }
                }
            }
        }

        // 如果所有节点都已访问，返回最大的距离值，否则返回 -1
        if (dist.size() != N) return -1;
        int ans = 0;
        for (const auto& kv : dist) {
            ans = max(ans, kv.second);
        }
        return ans;
    }
};

```
python:  
上文原理中将所有未更新节点都设为了无穷，实际代码中不一定需要这样设置，知识强调了如何更新节点的顺序，下例中用了最小堆，所有已经至少被更新了一次距离值的节点才会被更新最小堆中，而更‘远处’的节点一定还没有更新。  
```python
import heapq
from collections import defaultdict

class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        # 构建邻接表表示的图
        graph = defaultdict(list)
        for u, v, w in times:
            graph[u].append((v, w))

        # 使用优先队列作为最小堆存储距离值和节点
        pq = [(0, K)]
        # 用来存储源节点到其他节点的最短距离
        dist = {}

        while pq:
            time, node = heapq.heappop(pq)

            # 如果节点尚未访问，将其添加到 dist 中
            if node not in dist:
                dist[node] = time
                # 遍历邻接节点，将它们的距离值添加到优先队列中
                for next_node, edge_time in graph[node]:
                    if next_node not in dist:
                        heapq.heappush(pq, (time + edge_time, next_node))

        # 如果所有节点都已访问，返回最大的距离值，否则返回 -1
        if len(dist) != N:
            return -1

        return max(dist.values())
```

