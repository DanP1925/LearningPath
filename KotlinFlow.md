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

Collection of flow always happens in the context of the calling coroutine.
The exception is when using the *flowOn* operator.

For example:

```kotlin
    val flowA = flow{
        for (i in 1..3) {
            Thread.sleep(100) // pretend we are computing it in CPU-consuming way
            println("Emitting $i")
            emit(i) // emit next value
        }
    }.flowOn(Dispatchers.Default)

    runBlocking {
        flowA.collect{
            println("Collected $it")
        }
    }

```

Everything upstream flowOn runs on the background thread from Dispatchers.Default
And everything inside the runBlocking (the collect) runs on the main thread.
