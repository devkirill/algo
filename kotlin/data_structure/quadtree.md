# Дерево квадрантов

Используется класс [Point](../geometry/point.md) и [Rectangle](../geometry/rectangle.md)

```kotlin
class QuadTree(val rect: Rectangle) {
    private var container = listOf<QuadTree>()
    var value = false

    val leafs: List<QuadTree> get() = container
    val isLeaf: Boolean get() = leafs.isEmpty()

    operator fun contains(p: Point): Boolean = if (isLeaf) value else container.filter { p in it.rect }.any { p in it }

    private fun split() {
        if (!isLeaf)
            return
        val c = (rect.from + rect.to) / 2
        val list = mutableListOf(
            Rectangle(rect.from, c),
            Rectangle(Point(rect.from.x, rect.to.y), c + Point(0, 1)),
            Rectangle(Point(rect.to.x, rect.from.y), c + Point(1, 0)),
            Rectangle(rect.to, c + Point(1, 1))
        )
            .filter { it.to.x <= rect.to.x && it.to.y <= rect.to.y }
        if (list.isEmpty())
            return
        container = list
            .map { val res = QuadTree(it); res.value = value; res }
    }

    fun add(r: Rectangle) {
        if (rect in r) {
            value = true
            container = listOf()
        } else if (!rect.intersect(r)) {

        } else if (!isLeaf || !value) {
            split()
            container.forEach { it.add(r) }
            val f = container.first().value
            if (container.all { it.isLeaf && it.value == f }) {
                value = true
                container = listOf()
            }
        }
    }

    fun remove(r: Rectangle) {
        if (rect in r) {
            value = false
            container = listOf()
        } else if (!rect.intersect(r)) {

        } else if (!isLeaf || value) {
            split()
            container.forEach { it.remove(r) }
            val f = container.first().value
            if (container.all { it.isLeaf && it.value == f }) {
                value = false
                container = listOf()
            }
        }
    }

    override fun toString(): String {
        return ((rect.from.y)..(rect.to.y)).joinToString("\n", postfix = "\n") { y ->
            ((rect.from.x)..(rect.to.x)).joinToString("") { x ->
                if (Point(x, y) in this) "0" else "."
            }
        }
    }
}
```