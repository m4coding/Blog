# kotlin协程

    class ServerService : Service(), CoroutineScope {

        private val job = Job() //job主要用来控制协程的生命周期的状态的，有区分于SupervisorJob -> childCancelled

        /**
         * 重新定义coroutineContext,launcher时默认在Dispatchers.Default （即异步线程）
         */
        override val coroutineContext: CoroutineContext
            get() = Dispatchers.Default + job
    }

## 参考

[kotlin协程-Android实战](https://juejin.im/post/5d74ad56e51d456201486eab)

[官方的kotlin指导](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html)

[协程中的取消和异常 | 异常处理详解](https://blog.csdn.net/jILRvRTrc/article/details/107437548)
