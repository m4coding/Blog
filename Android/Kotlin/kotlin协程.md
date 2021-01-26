# kotlin协程

    class ServerService : Service(), CoroutineScope {

        private val job = Job() //job主要用来控制协程的生命周期的状态的，有区分于SupervisorJob -> childCancelled

        /**
         * 重新定义coroutineContext,launcher时默认在Dispatchers.Default （即异步线程）
         */
        override val coroutineContext: CoroutineContext
            get() = Dispatchers.Default + job
    }


## 问题点

1、有个问题，当在协程launch里面发生代码异常时，若导致了应用闪退，这个时候看到log，是看不到应用闪退的具体信息。。。可能通过CoroutineExceptionHandler来处理来捕捉异常

    val handler = CoroutineExceptionHandler { context, exception ->
        Log.e(TAG, "Caught $exception")
    }
    launch {
        val job = GlobalScope.launch(handler) {
            Log.d(TAG, "Throwing exception from launch")
            throw IndexOutOfBoundsException() 
        }
        job.join()
    }

## 参考

[kotlin协程-Android实战](https://juejin.im/post/5d74ad56e51d456201486eab)

[官方的kotlin指导](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html)

[协程中的取消和异常 | 异常处理详解](https://blog.csdn.net/jILRvRTrc/article/details/107437548)
