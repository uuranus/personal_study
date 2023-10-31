``` kotlin
import java.util.*

interface Tree<E> {

    fun root(): E

    fun parent(node: E): E
    fun children(parent: E): Iterable<E>

    fun isInternal(node: E): Boolean = numChildren(node) == 0
    fun isExternal(node: E): Boolean = numChildren(node) > 0

    fun isRoot(node: E): Boolean = node == root()
    fun isEmpty(): Boolean = size() == 0

    fun numChildren(node: E): Int
    fun size(): Int

    fun depth(node: E): Int
    fun height(node: E): Int

    //dfs
    fun preorder(node: E) {
        println(node)
        for (child in children(node)) {
            preorder(child)
        }
    }

    fun postorder(node: E) {
        for (child in children(node)) {
            preorder(child)
        }
        println(node)
    }

    //bfs
    fun levelOrder(node: E) {
        val q: Queue<E> = LinkedListQueue()

        while (!q.empty()) {
            val cur = q.dequeue()
            println(cur)
            for (child in children(cur)) {
                q.enqueue(child)
            }
        }
    }
}

```
