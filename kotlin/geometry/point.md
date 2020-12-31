# Реализация целочисленного класса для точки

```kotlin
data class Point(var x: Int, var y: Int) {
    val length: Double get() = Math.sqrt((x * x + y * y).toDouble())

    constructor(x: Double, y: Double) : this(toInt(x), toInt(y))

    operator fun plus(v: Point) = Point(x + v.x, y + v.y)

    operator fun plusAssign(v: Point) {
        x += v.x
        y += v.y
    }

    operator fun minus(v: Point) = Point(x - v.x, y - v.y)

    operator fun minusAssign(v: Point) {
        x -= v.x
        y -= v.y
    }

    operator fun times(t: Int) = Point(x * t, y * t)

    operator fun times(t: Double) = Point(x * t, y * t)

    operator fun timesAssign(t: Int) {
        x *= t
        y *= t
    }

    operator fun timesAssign(t: Double) {
        x = toInt(x * t)
        y = toInt(y * t)
    }

    operator fun div(t: Int) = Point(x / t, y / t)

    operator fun div(t: Double) = Point(x / t, y / t)

    operator fun divAssign(t: Int) {
        x /= t
        y /= t
    }

    operator fun divAssign(t: Double) {
        x = toInt(x / t)
        y = toInt(y / t)
    }

    companion object {
        fun toInt(d: Double): Int = (d + 1e-9).toInt()
    }
}
```