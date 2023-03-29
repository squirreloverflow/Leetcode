算法的基本思想是通过将顶点作为中间节点来更新所有顶点对之间的距离。初始化时，距离矩阵使用边的权重填充。对于不存在的边，权重为无穷大（表示不可达）。该算法适合解决所有顶点边的问题。

算法步骤如下：

初始化距离矩阵
遍历所有顶点，将其作为中间节点
对于每对顶点（i，j），检查通过中间节点是否可以得到更短的路径
如果可以，更新距离矩阵  

一个N x N的网格(grid) 代表了一块樱桃地，每个格子由以下三种数字的一种来表示：

0 表示这个格子是空的，所以你可以穿过它。
1 表示这个格子里装着一个樱桃，你可以摘到樱桃然后穿过它。
-1 表示这个格子里有荆棘，挡着你的路。
你的任务是在遵守下列规则的情况下，尽可能的摘到最多樱桃：

从位置 (0, 0) 出发，最后到达 (N-1, N-1) ，只能向下或向右走，并且只能穿越有效的格子（即只可以穿过值为0或者1的格子）；
当到达 (N-1, N-1) 后，你要继续走，直到返回到 (0, 0) ，只能向上或向左走，并且只能穿越有效的格子；
当你经过一个格子且这个格子包含一个樱桃时，你将摘到樱桃并且这个格子会变成空的（值变为0）；
如果在 (0, 0) 和 (N-1, N-1) 之间不存在一条可经过的路径，则没有任何一个樱桃能被摘到。

```python
from typing import List

def floyd_warshall(graph: List[List[int]]) -> List[List[int]]:
    n = len(graph)
    dist = [row[:] for row in graph]

    # Floyd-Warshall Algorithm

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][k] != float('inf') and dist[k][j] != float('inf') and dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    return dist


def main():
    graph = [
        [0, 5, float('inf'), 10],
        [float('inf'), 0, 3, float('inf')],
        [float('inf'), float('inf'), 0, 1],
        [float('inf'), float('inf'), float('inf'), 0]
    ]

    result = floyd_warshall(graph)

    for row in result:
        print(['INF' if val == float('inf') else val for val in row])

if __name__ == "__main__":
    main()
```
