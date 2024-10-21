

```kotlin
class LinearProbing<K, V> {
    private val TABLE_SIZE = 13
    private val table = Array<Any?>(TABLE_SIZE) {}
    private val values = Array<Any?>(TABLE_SIZE) {}

    private fun hash(k: K): Int {
        return k.hashCode().and(0x7ffffff) % TABLE_SIZE
    }

    private fun findSlot(hash: Int, key: K): Int {
        var start = hash

        do {
            if (table[start] == key || table[start] == null) return start
            start = (start + 1) % TABLE_SIZE
        } while (start != hash)

        return -1
    }

    private fun put(key: K, value: V) {
        val h = hash(key)
        val idx = findSlot(h, key)

        if (table[idx] == key) {
            values[idx] = value
        }

        if (table[idx] == null) {
            table[idx] = key
            values[idx] = value
        }
    }

    private fun get(key: K) {

    }

    private fun remove(key: K) {

    }
}

```
