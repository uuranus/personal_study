비선형 자료구조

# Tree
- 부모-자식 관계로 계층적으로 데이터를 구성하고 있는 자료구조
- ex. 파일 시스템

**용어**
- 노드(node) -> 데이터와 다른 노드의 레퍼런스를 가지고 있는 구조체
- 루트 노드 -> 최상위 노드
- 리프 노드 -> 최하단 자식이 없는 노드
- degree -> 트리 전체에서 노드가 가진 자식의 수
- depth -> 트리의 깊이. 루트 노드가 0이고 리프노드로 갈수록 1 증가
- height -> 트리의 높이. 제일 긴 depth가 트리의 높이
- level -> 루트부터 특정 노드까지의 깊이


### 트리의 종류
- n-ary Tree
  - 한 노드가 자식을 n개 가질 수 있는 일반적인 트리
- Binary tree
  - 한 노드가 자식을 최대 2개 가질 수 있는 트리
- Balanced Tree
  - 트리는 한 쪽 방향으로 노드들이 쏠릴 수 있다 (skewed tree)
  - 그럼 O(n)시간으로 탐색 시간이 늘어나기에 트리의 balance를 유지해 O(logN)의 탐색시간을 유지할 수 있음
  - 트리 전체에서 왼쪽과 오른쪽 서브트리의 높이 차이가 1 이하인 트리는 Balanced Tree라고 한다.
  - ex. Red-Black Tree (Red, Black의 규칙에 맞춰 균형 유지), AVL (left, right rotate를 통해 균형 유지)
 
### Binary Tree
-  한 노드가 자식을 최대 2개 가질 수 있는 트리
-  Perfect Binary Tree (정 이진 트리)
    - 자식을 0개 or 2개 가지고 있는 이진 트리
-  Full Binary Tree (포화 이진트리)
    - 리프노드를 제외하고 모든 노드들이 자식을 2개씩 가지고 있는 이진 트리
-  Complete Binary Tree (완전 이진트리)
    - 마지막 레벨을 제외하고 포화 이진트리이고 마지막 레벨은 왼쪽에서부터 노드가 채워지는 방식
    - 완전 이진트리의 마지막 레벨도 꽉 차면 포화 이진트리가 된다.
    - Heap 구현 시 사용
-  Binary Search Tree (이진 탐색 트리)
    - 현재 노드를 기준으로 왼쪽 서브트리는 현재 노드보다 작은 값, 오른쪽 서브트리는 현재 노드보다 큰 값으로 정렬되어 있는 이진 트리
    - inorder 방식으로 traversal하면 정렬된 값을 얻을 수 있다.
 
**traversal**

- 이진 트리를 순회하는 방법
- 현재 노드를 언제 방문하느냐에 따라서 순회방법이 나뉘어짐
      1
     / \
    2   3
구조라면
- preorder -> 1 -> 2 -> 3 순으로 순회하는 방식
- inorder -> 2 -> 1 -> 3 순으로 순회하는 방식
- postorder -> 2 -> 3 -> 1 순으로 순회하는 방식
- level order -> 노드 - 노드의 자식 노드 - 노드의 자식노드의 자식노드 순으로 레벨별로 순회하는 방식. Queue를 이용해 구현


# Graph
- 데이터를 가지고 있는 정점(Vertex)과 정점을 잇는 간선(Edge)으로 이루어진 비선형 자료구조
- 간선의 방향성이 있으면 directed graph, 없으면 undirected graph라고 한다.
- 간선에 가중치가 있으면 weighted graph, 없으면 Unweighted graph라고 한다.
- 그래프 구현 방법
  - adjacency List
    - 각 정점이 연결된 정점들을 리스트로 표현. (공간 효율적)
  - adjacency Matrix
    - 정점 간의 연결을 2차원 배열로 표현. (빠른 접근 가능하지만 공간 비효율적)
    - [i][j]는 i번 정점과 j번 정점의 간선 유무를 나타냄



## DFS (Depth-First Search)
- 방문할 수 있을 때까지 정점들과 연결된 간선들을 따라가는 방식
- stack or 재귀를 이용하여 구현
- 정점과 간선을 한 번씩 방문하므로 O(v+e)시간이 소요됨


## BFS (Breadth-First Search)
- 노드와 연결된 모든 노드들을 방문한 후 그 노드들의 다음 노드를 방문하는 방식
- queue 이용하여 구현
- 정점과 간선을 한 번씩 방문하므로 O(v+e)시간이 소요됨
- 최단거리를 구할 때 사용

  
