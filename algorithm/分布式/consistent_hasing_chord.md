## 一致性hash(Chord) 《Chord: A Scalable Peer-to-peer Lookup Service for Internet Applications》

### 概念
 1. 给定整数m，数据分在2^m个哈希值上。2^m个点顺序链接，2^m - 1与0相连形成哈希环。
 2. 区间[a,b]代表环上的a到b顺时针方向经过的所有点。
 3. 数据节点node被随机的分到哈希环上，节点的最近的下一个节点称为节点的successor，最近的前一个节点称为predecessor。
 4. 每个node所管理的哈希值为(predecessor, node]。
 5. 任意一哈希值k对应的管理节点称为manager(k)。
 6. 集群中的node节点数为N。

### 查找
在任意一节点上，给定key对应的哈希值k，找到manager(k)。

一般做法是根据node节点的successor依次查找，这样节点的交互次数为O(N)。

Chord提供了一种更快的方法，其节点间的交互次数为O(lg(N))。

每个node节点维护一个m大小的数组，每个数组项记录距离node为2^i的key的manager节点(0 <= i <= m-1)。

查找时，1. 如果k在(node, node.successor]中，则node.successor即是manager(k)，2. 否则在该节点的数组中查找在k之前的最大(即i值最大)的node，重复1步骤。

### 节点加入
根据性能和正确性有两种方法，一种不考虑并发加入但操作很快的方法，另一种是考虑并发加入但操作比较慢的方法。

假设插入的节点为n，在任意一节点n'上执行。

不考虑并发的情况下，基本的思想为根据n'找到manager(n)，然后初始化n的数组，更改其他node节点的数组，最后通知上层迁移数据。

考虑并发的情况下，根据n'找到manager(n)，然后初始化n的successor节点。所有的节点都定期执行stable检测：根据successor.predecessor判断是否需要更新本节点的successor，然后将本节点发送给successor，看successor是否需要更新其predecessor。

