# Прямоугольник

Используется класс [Point](point.md)

```kotlin
data class Rectangle private constructor(val from: Point, val to: Point) {
    val p1: Point = Point(from.x, to.y)
    val p2: Point = Point(to.x, from.y)

    operator fun contains(p: Point) = from.x <= p.x && p.x <= to.x && from.y <= p.y && p.y <= to.y

    operator fun contains(rect: Rectangle) = rect.from in this && rect.to in this

    fun intersect(rect: Rectangle) =
        rect.from.x <= to.x && from.x <= rect.to.x && rect.from.y <= to.y && from.y <= rect.to.y

    companion object {
        operator fun invoke(from: Point, to: Point): Rectangle {
            return Rectangle(
                Point(Math.min(from.x, to.x), Math.min(from.y, to.y)),
                Point(Math.max(from.x, to.x), Math.max(from.y, to.y))
            )
        }
    }
}
```