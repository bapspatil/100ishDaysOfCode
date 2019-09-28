
# ObjectPool, Prototype & Factory

This file is about how ObjectPool, Prototype and Factory can be implemented in Kotlin.

## ObjectPool

```
// Extend this abstract class to create an ObjectPool
internal abstract class ObjectPool<T> {
  var deadTime:Long = 0
  var lock:Hashtable<T, Long>
  var unlock:Hashtable<T, Long>
  init{
    deadTime = 50000 // 50 seconds
    lock = Hashtable<T, Long>()
    unlock = Hashtable<T, Long>()
  }
  internal abstract fun create():T
  internal abstract fun validate(o:T):Boolean
  internal abstract fun dead(o:T)
  @Synchronized fun takeOut():T {
    val now = System.currentTimeMillis()
    val t:T
    if (unlock.size() > 0)
    {
      val e = unlock.keys()
      while (e.hasMoreElements())
      {
        t = e.nextElement()
        if ((now - unlock.get(t)) > deadTime)
        {
          // object has deadd
          unlock.remove(t)
          dead(t)
          t = null
        }
        else
        {
          if (validate(t))
          {
            unlock.remove(t)
            lock.put(t, now)
            return (t)
          }
          else
          {
            // object failed validation
            unlock.remove(t)
            dead(t)
            t = null
          }
        }
      }
    }
    // no objects available, create a new one
    t = create()
    lock.put(t, now)
    return (t)
  }
  @Synchronized fun takeIn(t:T) {
    lock.remove(t)
    unlock.put(t, System.currentTimeMillis())
  }
}
```

## Prototype

```
internal abstract class Color : Cloneable {
  protected var colorName:String
  internal abstract fun addColor()
  public override fun clone():Any {
    val clone:Any = null
    try
    {
      clone = super.clone()
    }
    catch (e:CloneNotSupportedException) {
      e.printStackTrace()
    }
    return clone
  }
}
internal class blueColor:Color() {
  init{
    this.colorName = "blue"
  }
  internal override fun addColor() {
    println("Blue color added")
  }
}
internal class blackColor:Color() {
  init{
    this.colorName = "black"
  }
  internal override fun addColor() {
    println("Black color added")
  }
}
internal object ColorStore {
  private val colorMap = HashMap<String, Color>()
  init{
    colorMap.put("blue", blueColor())
    colorMap.put("black", blackColor())
  }
  fun getColor(colorName:String):Color {
    return colorMap.get(colorName).clone() as Color
  }
}
// Driver class
internal object Prototype {
  @JvmStatic fun main(args:Array<String>) {
    ColorStore.getColor("blue").addColor()
    ColorStore.getColor("black").addColor()
    ColorStore.getColor("black").addColor()
    ColorStore.getColor("blue").addColor()
  }
}
```

## Factory

### CarType.kt

```
enum class CarType {
  SMALL, SEDAN, LUXURY
}
```

### Car.kt

```
abstract class Car(model:CarType) {
  var model:CarType = null
  init{
    this.model = model
    arrangeParts()
  }
  private fun arrangeParts() {
    // Do one time processing here
  }
  // Do subclass level processing in this method
  protected abstract fun construct()
}
```

### LuxuryCar.kt

```
class LuxuryCar internal constructor() : Car(CarType.LUXURY) {
  init{
    construct()
  }
  protected fun construct() {
    println("Building luxury car")
    // add accessories
  }
}
```

### SmallCar.kt

```
class SmallCar internal constructor() : Car(CarType.SMALL) {
  init{
    construct()
  }
  protected fun construct() {
    println("Building small car")
    // add accessories
  }
}
```

### SedanCar.kt

```
class SedanCar internal constructor() : Car(CarType.SEDAN) {
  init{
    construct()
  }
  protected fun construct() {
    println("Building sedan car")
    // add accessories
  }
}
```

### CarFactory.kt

```
object CarFactory {
  fun buildCar(model:CarType):Car {
    val car:Car = null
    when (model) {
      SMALL -> car = SmallCar()
      SEDAN -> car = SedanCar()
      LUXURY -> car = LuxuryCar()
      else -> {}
    }
    return car
  }
}
```
