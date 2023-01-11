# Kotlin Flow

An asynchronous data stream that sequentially emits values.
It can return mutiple asynchronously computed values instead of a single value like with suspend functions.
It can be created with the *flow* builder.
Whenever a *flow* builder is called it expects an *emit*.
Flows are **cold** streams, it doesn't run until it is collected.
It is useful because it doesn't block the thread where it is being called and instead it can be suspended.

A simple example:

```kotlin
fun main(){
    fun simple(): Flow<Int> = flow {
        for (i in 1..3) {
            delay(100)
            emit(i)
        }
    }
    
    runBlocking {
      launch {
        for (k in 1..3) {
          println("I'm not blocked $k")
          delay(100)
        }
      }
      simple().collect { value -> println(value) }
    }

}
```

