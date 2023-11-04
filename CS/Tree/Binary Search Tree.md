# Binary Tree

> 최대 2개의 자식노드를 가질 수 있는 트리
> 

# Binary Search Tree

> 현재 노드의 값이 왼쪽 subtree의 노드들보다 크고 오른쪽 subtree의 노드들보다 작은 binary tree
> 

## Search

- 루트노드를 시작으로 찾고자 하는 값이 현재 노드보다 작다면 왼쪽 subtree로 이동, 크다면 오른쪽 subtree로 이동.
- 리프노드에 도달한 경우 -1 return

## Insertion

- search와 동일하게 넣을 값이 현재 값보다 작다면 왼쪽 sub tree로, 크다면 오르쪽 subtree로 이동한다.
- 리프노드에 도달하면 그 위치에 새로운 노드를 삽입

## Deletion

- 삭제할 노드가 리프노드인 경우
    - 그 노드를 삭제해버리면 된다.
- 삭제할 노드가 child가 하나 있는 경우
    - 나의 부모 노드와 나의 child 노드를 연결한다.
- 삭제할 노드가 child가 두개 있는 경우
    - 나의 오른쪽 subtree에서 제일 작은 값, A를 찾는다.
    - A의 값으로 현재 노드를 변경한다.
    - A를 삭제한다.

## Runtime

- O(depth)
    - worst O(n), best O(logN)

## Duplicate Value

- 중복을 허용하지 않는다면? - 중복이 존재하면 그냥 종료
- 중복을 허용한다면? - right sub tree로 삽입
    - inorder로 순회할 때 ordered한 순서로 출력이 잘 될 것
