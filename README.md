#1 交替打印数字和字母
##1.1 问题描述
使用两个 goroutine 交替打印序列，一个 goroutinue 打印数字， 另外一个goroutine打印字母， 最终效果如下: 12AB34CD56EF78GH910IJ 。

##1.2 解题思路
>问题很简单，使用 channel 来控制打印的进度。使用两个 channel，来分别控制数字和字母的打印序列， 数字打印完成后通过 channel 通知字母打印, 
字母打印完成后通知数字打印，然后周而复始的工作。

##1.3 关键点
这个问题不难，关键是要注意防止死锁。为什么 `chan_c` 需要缓存，而 `chan_n` 不需要呢?
当两个打印 goroutine 无限交替运行时，没有缓存是OK的，但很明显上面的代码不是，当打印数字的 goroutine 先退出，也就没有了 goroutine 来读取 `chan_c` 中的内容了， 而打印字母的goroutine就会阻塞在 `chan_c <- true` ，这样就导致了死锁。
因此，为了防止死锁，需要将`chan_c`设计成带缓冲的channel。


