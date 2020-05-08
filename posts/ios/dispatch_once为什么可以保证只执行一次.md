# dispatch_once 为什么可以保证只执行一次？

地址：https://shevakuilin.com/interview-dispatch_once

创建时间：2020-05-08

## 简介

想尝试写一个面试回答的系列，这是第一篇。

GCD 中的 dispatch_once 在面试中是一个比较高频的出现的考察点，这篇文章以面试的角度来回答，为什么 dispatch_once 可以保证只执行一次

## 正文

这是一道面试真题，面试官提出了一个问题，我们来尝试回答一下。

`dispatch_once` 为什么可以保证只执行一次？

1. `dispatch_once_f` 实现原理是什么样的？
2. `dispatch_once` 中的原子性操作是怎样的？
3. `vval` 代表什么？ `DISPATCH_ONCE_DONE` 又表示什么？
4. 和 `@synchronized` 的优劣分析？

我们以 Q & A 问答的形式来回答面试官的这个问题。

## 为什么可以保证只执行一次

Q: `dispatch_once`  为什么可以保证只执行一次？

A: `dispatch_once`  封装并执行了 `dispatch_once_f` 函数，其内部使用原子性操作进行标记，以此来配合信号量来决定是否唤醒其他等待的线程，而信号量则用来确保同一时间只有一个线程可以执行回调。

## dispatch_once_f 实现原理是什么样的？

为了便于理解，先放上 `dispatch_once_f` 的源码

```objective-c
// Block 数据结构
struct Block_layout {
    // 指向表明该block类型的类
    void *isa;
    // 按bit位表示一些 block 的附加信息，比如判断 block 类型、判断 block 引用计数、判断 block 是否需要执行辅助函数等
    int flags;
    // 保留变量
    int reserved;
    // 函数指针，指向具体的 block 实现的函数调用地址
    void (*invoke)(void *, ...);
    struct Block_descriptor *descriptor;
    /* Imported variables. */
};

// 宏定义
// 触发 block 的实现
#define _dispatch_Block_invoke(bb) \
        ((dispatch_function_t)((struct Block_layout *)bb)->invoke)

// 入口方法
void dispatch_once(dispatch_once_t *val, dispatch_block_t block) {
    dispatch_once_f(val, block, _dispatch_Block_invoke(block));
}

#define DISPATCH_ONCE_DONE ((struct _dispatch_once_waiter_s *)~0l)

struct _dispatch_once_waiter_s {
    //链表下一个节点
    volatile struct _dispatch_once_waiter_s *volatile dow_next;
    // 信号量
    _dispatch_thread_semaphore_t dow_sema;
};

void dispatch_once_f(dispatch_once_t *val, void *ctxt, dispatch_function_t func) {
  	// volatileg 关键字编辑的变量 vval
    // 告诉编译器此指针指向的值随时可能被其他线程改变
    // 从而使得编译器不对此指针进行代码编译优化
    struct _dispatch_once_waiter_s * volatile *vval =
            (struct _dispatch_once_waiter_s**)val;
    struct _dispatch_once_waiter_s dow = { NULL, 0 };
    struct _dispatch_once_waiter_s *tail, *tmp;
  	// 声明信号变量
    _dispatch_thread_semaphore_t sema;

    if (dispatch_atomic_cmpxchg(vval, NULL, &dow, acquire)) {
        _dispatch_client_callout(ctxt, func);

        dispatch_atomic_maximally_synchronizing_barrier();
        // above assumed to contain release barrier
        tmp = dispatch_atomic_xchg(vval, DISPATCH_ONCE_DONE, relaxed);
        tail = &dow;
        while (tail != tmp) {
            while (!tmp->dow_next) {
                dispatch_hardware_pause();
            }
            sema = tmp->dow_sema;
            tmp = (struct _dispatch_once_waiter_s*)tmp->dow_next;
            _dispatch_thread_semaphore_signal(sema);
        }
    } else {
        dow.dow_sema = _dispatch_get_thread_semaphore();
        tmp = *vval;
        for (;;) {
            if (tmp == DISPATCH_ONCE_DONE) {
                break;
            }
            if (dispatch_atomic_cmpxchgvw(vval, tmp, &dow, &tmp, release)) {
                dow.dow_next = tmp;
                _dispatch_thread_semaphore_wait(dow.dow_sema);
                break;
            }
        }
        _dispatch_put_thread_semaphore(dow.dow_sema);
    }
}
```

Q: `dispatch_once_f` 实现原理是什么样的？

A: 其内部定义了多个 `_dispatch_once_waiter_s ` 结构体和一个 `_dispatch_thread_semaphore_t `  信号量，通过原子性操作 `dispatch_atomic_cmpxchg `  来判断标记值 `vval`  是否为 NULL (首次调用 `dispatch_once` 时，因为外部传入的 `dispatch_once_t` 变量值为 nil，所以 `vval` 会为NULL) ，如果为 NULL，则调用 `_dispatch_client_callout` 来执行回调，然后在回调执行完成之后，将 `vval` 的值更新成 `DISPATCH_ONCE_DONE` (表示任务已完成)，最后，对链表的节点进行遍历，并调用 `_dispatch_thread_semaphore_signal` 来唤醒等待中的信号量。

因为`dispatch_atomic_cmpxchg`是原子性操作，所以只有一个线程进入到该逻辑分支中，其他线程会进入另一个分支。

如果不为 NULL 或其他线程同时也调用 `dispatch_once` 时，会判断回调是否 **已标记完成** ，如果已完成则跳出循环；否则就是更新链表并调用 `_dispatch_thread_semaphore_wait` 阻塞线程，等待回调被标记完成后，再唤醒当前等待的线程。

## dispatch_once 中的原子性操作是怎样的？

Q: `dispatch_once` 中的原子性操作是怎样的?

A: 原子性操作是 `dispatch_atomic_cmpxchg(vval, NULL, &dow, acquire)` ，会将 `$dow` 赋值给 `vval` ，如果 `vval` 的初始值为NULL，返回 `YES` ,否则返回 `NO` 。以及dispatch_atomic_xchg(vval, DISPATCH_ONCE_DONE)` 将 `vval` 修改为指定状态 `DISPATCH_ONCE_DONE。

## vval 代表什么？ DISPATCH_ONCE_DONE 又表示什么？

Q: `vval` 代表什么？ `DISPATCH_ONCE_DONE` 又表示什么？

A: `vval` 可以理解为标记值， `DISPATCH_ONCE_DONE` 用来标记回调是否已完成，以此来决定是否要唤起信号量来解除线程的阻塞。

## 和 @synchronized 的优劣分析？

Q: 和 `synchronized` 的优劣分析？

A: 相比之下 `dispatch_once` 的性能更高，速度更快，并且针对处理器进行了优化。两者分别利用来不同的方式来保证线程安全， `synchronized` 采用的是递归互斥锁的方式来保证线程安全，而 `dispatch_once` 是使用原子操作来代替锁，使用信号量来保证线程同步。