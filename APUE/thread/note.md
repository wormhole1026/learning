# 11. 线程

## 11.2 线程概念

## 11.3 线程标识

` int pthread_equal(pthread_t tid1, pthread_t tid2);

` pthread_t pthread_self(void);

## 11.4 线程创建

` int pthread_create(pthread_t *restrict tidp, const pthread_attr_t *restrict attr, void *(*start_rtn)(void), void *restrict arg);

成功返回0, 失败返回错误编号

## 11.5 线程终止

退出进程:

1. 任一线程调用exit, _Exit, _exit
2. 中止进程的信号

退出线程:

1. 线程只是从启动例程中返回, 返回值是线程的退出码
2. 线程可以被同一进程中的其它线程取消
3. 线程调用pthread_exit

` void pthread_exit(void *rval_ptr);

pthread_join可以访问到rval_ptr, pthread_join是阻塞调用

` int pthread_join(pthread_t thread, void **rval_ptr);

1. 线程从启动例程中返回, rval_ptr将包含返回码
2. 线程被取消, rval_ptr将指向PTHREAD_CANCELED
3. pthread_exit: rval_ptr指向pthread_exit的rval_ptr

可以通过pthread_join将线程置为分离状态, 对于已经分离的线程, pthread_join会失败并返回EINVAL

若不感兴趣可以join的时候rval_ptr可以设置为NULL

请求取消同一进程中的其它线程

` int pthread_cancel(pthread_t tid);

pthread_cancel会使得线程表现为如同调用了参数为PTHREAD_CANCELED的pthread_exit函数
pthread_cancel并不等待线程终止, 只是发出请求 

线程清理处理程序

` void pthread_cleanup_push(void (*rtn)(void*), void *arg);

` void pthread_cleanup_pop(int execute);

触发:

1. 调用pthread_exit时
1. 响应pthread_cancel时
1. 用非0 execute参数调用pthread_cleanup_pop时

正常退出例程是不会触发pthread_cleanup的

线程分离

` int pthread_detach(pthread_t tid); 

## 11.6 线程同步

* 互斥量

pthread_mutex_t

初始化:

1. 静态初始化: PTHREAD_MUTEX_INITIALIZER
1. 动态分配: 

` int pthread_mutex_init();

` int pthread_mutex_destroy();

操作:

` lock, trylock, unlock

* 避免死锁

1. 顺序加锁
1. trylock不到的时候释放掉已持有的锁过一会再做尝试

* 读写锁

` pthread_rwlock_init, pthread_rwlock_destroy

` pthread_rwlock_rdlock, pthread_rwlock_wrlock, pthread_rwlock_unlock, pthread_rwlock_tryrdlock, pthread_rwlock_trywrlock

有返回值, 最好检查一下返回值

* 条件变量

` pthread_cond_init, pthread_cond_destroy 

` pthread_cond_wait, pthread_cond_timedwait

` pthread_cond_signal, pthread_cond_broadcast




