# Tree

> Key와 Children을 가지는 자료구조
> 
- syntax tree, hierachy (생물학적 관계도) 같은 데에서 사용

# Terminology

- root - 최상위 노드
- level - root와 node 사이의 node의 개수 + 1. 루트는 level 1
- depth - 루트와의 거리 +1. 루트는 depth 1.
- height - 제일 큰 depth의 길이. 최하위 리프 노드의 depth
- leaf node - 자식이 없는 노드. 최하위 노드
- interior node - 자식이 있는 노드
- Ascendants - 조상 노드 (parent + parent의 parent까지 포함)
- Descendants - 자손 노드 (children + children의 children까지 포함)
- Sibiling - 같은 부모를 가지는 노드들

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
- level-traversal이라고도 함

```kotlin
if(tree == null) return

q:Queue = LinkedList<>()

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
