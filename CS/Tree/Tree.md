# Tree

> 데이터를 가지고 있는 node가 parent-child 관계로 계층적으로 저장되어 있는 비선형적 자료구조
> 
- syntax tree, hierachy (생물학적 관계도) 같은 데에서 사용

# Terminology

- root - 최상위 노드
- level - root와 node 사이의 node의 개수 + 1. 루트는 level 1
- depth - 루트와의 거리. 루트는 depth 0.
- height - 제일 큰 depth의 길이. 최하위 리프 노드의 depth
- leaf node - 자식이 없는 노드. 최하위 노드. exterior node라고도 함
- interior node - 자식이 있는 노드
- Ascendants - 조상 노드 (parent + parent의 parent까지 포함)
- Descendants - 자손 노드 (children + children의 children까지 포함)
- Sibiling - 같은 부모를 가지는 노드들
- degree - 자식노드 수 (binary tree는 degree가 최대 2)
- subtree - 자신과 자신의 후손노드들로 구성된 트리

# Traversal (Walking Tree)

### depth-first (DFS)

- inorder

```kotlin
if(tree == null) return

inOrderTraversal(tree.left)
print(tree.key)
inOrderTraversal(tree.right)
```

- preorder

```kotlin
if(tree == null) return

print(tree.key)
preOrderTraversal(tree.left)
preOrderTraversal(tree.right)
```

- postorder

```kotlin
if(tree == null) return

postOrderTraversal(tree.left)
postOrderTraversal(tree.right)
print(tree.key)
```

- recursive하게 실행되므로 stackOverflow나지 않게 조심

### breadth-first (BFS)

- level별로 순환
- level-order이라고도 함

```kotlin
if(tree == null) return

val q:Queue = LinkedList<>()

q.add(tree)

while(q.isNotEmpty()){
	Node node = q.poll()
	print(node)
	if(node.left != null){
		q.add(node.left)
	}

	if(node.right != null){
		q.add(node.right)
	}
}
```

### 수행시간

- BFS, DFS 둘 다 노드를 한 번씩 방문하고 종료하므로 O(n)
