### 竞争条件和临界区的定义

- 竞争条件：多个进程同时尝试改变变量的值，产生冲突
- 临界区：每次只能有一个进程进入并更改数据的代码段，避免**竞争条件**



### 临界区调度的条件（解决方案）

- 互斥：每次只能有一个进程将进入临界区
- 前进：当临界区为空且有进程期待进入时，进入临界区进程的选择不能无限期推迟
- 有限等待：进程提交访问申请到进程获得访问权限之间，其他进程进入临界区的次数是有限的



### 进程同步的典型结构

- 进入区、临界区、退出区、剩余区



### 彼得森算法（软件方案）及其约束

- 假设加载和存储的指令是原子的
- 共享变量：turn和flag[2]，turn指示轮到哪个进程进入临界区，flag指示某进程是否准备好进入临界区

```c++
while(true) {
	flag[i] = true;
	turn = j;
	while(flag[j] && turn == j);
	//critical section
	flag[i] = false;
	//remainder section
}
```

- 约束：为了提高系统性能，处理器和编译器会对没有依赖性的读写操作进行重排，当重排发生在具有共享数据的多线程中，可能会导致数据不一致的结果。



### 同步硬件的分类

- 只用一个核，能够防止被抢占和中断
- 原子硬件指令（不可中断）：测试和设置指令、交换指令
- 原子变量



### 测试和设置指令

```c++
bool test_set(int i) {
	if(i == 0) {
		i = 1;
		return true;
	}
	else return false;
}
```

```c++
while(true) {
    while(!test_set(bolt));		//busy waiting
    //critical section
    bolt = 0;
    //remainder section
}
```



### 交换指令

```c++
bool exchange(int register, int memory) {
	int temp;
    temp = memory;
    memory = register;
    register = temp;
}
```

```c++
while(true) {
    keyi = 1;
    while(keyi != 0) exchange(keyi, bolt);
    //critical section
    exchange(keyi, bolt);
    //remainder section
}
```



### 临界区调度的缺点（忙等待的危害）

- 忙等待，进程占用CPU但是没有任务的推进，浪费CPU时间
- 将测试的责任推给各个竞争的进程会削弱系统的可靠性，加重了用户的编程负担



### 互斥锁的原理和指令

- 使用acquire获取锁，获取成功后将锁设置为不可用
- 使用release释放锁，将锁设为可用状态



### 互斥锁的分类及其与多道程序或多核系统的关系

- 互斥锁的分类：自旋锁和互斥锁
- 自旋锁的acquire操作需要忙等待获取锁的状态，浪费了大量的CPU时间，在多道程序系统会形成阻塞
- 因为忙等待不需要上下文切换，因此在多核系统中，自旋锁在某一个CPU上忙等待，其他的进程同样可以进入临界区操作或者进行忙等待的操作



### 信号量的指令及其原理（PV原语）

- 需要保证不能同时对同一个信号量执行wait和signal操作
- 每个信号量都有一个关联的等待队列和数值value
- 访问临界区前使用wait指令，若value自减后的数值小于0，则使用系统调用block将当前进程添加进等待队列中，同时将当前进程阻塞；离开临界区前使用signal指令，若自加后的value大于等于0，则将某个进程从等待队列中移出，同时使用系统调用wakeup将该进程唤醒，放入就绪队列

```c++
wait(S) {
	value--;
	if(value < 0) {
		//add this process to waiting queue
		block();
	}
}
```

```c++
signal(S) {
	value++;
	if(value >= 0) {
		//remove a process P from the waiting queue
		wakeup(P);
	}
}
```



### 信号量的分类与用途

- 二元信号量：用于两个进程的同步，与互斥锁类似
- 计数信号量：用于多个进程的同步，正的信号量表示可以进入临界区的剩余进程数，负的信号量表示因为临界区已满而被放置在阻塞队列中的进程数量



### 信号量的使用

- 访问临界区前使用wait指令，若value自减后的数值小于0，则将当前进程添加进等待队列中，同时将当前进程阻塞；离开临界区前使用signal指令，若自加后的value大于等于0，则将某个进程从等待队列中移出，同时将该进程唤醒，放入就绪队列

```c++
semaphore S;	//initialized to 1
wait(S);
//critical section
signal(S);
//remainder section
```



### 实现原语的方法及优劣

- 因为不能同时对信号量执行wait和signal操作，因此需要把wait和signal放入临界区中，产生临界区问题
- 在单核系统上，可以在wait和signal操作执行期间禁止中断。因为一旦中断被禁止，不同进程的指令就不能交错，正在运行的进程能完整执行PV操作
- 在多核系统上，可以利用自旋锁来解决wait和signal的临界区问题。因为多核系统上禁止所有CPU的中断非常困难，且会严重降低性能。多核系统上利用自旋锁可以避免大量的上下文切换，节省系统调用产生的开销，此外，如果临界区很少被占用，则不会产生很多的忙等待。



### 信号量操作没有完全避免忙等待的原因

- 因为不能同时对信号量执行wait和signal操作，因此需要把wait和signal放入临界区中，临界区会产生忙等待，因此信号量操作没有完全避免忙等待



### 信号量数值的含义

- 正的信号量表示可以进入临界区的剩余进程数
- 负的信号量表示因为临界区已满而被放置在阻塞队列中的进程数量



### 信号量操作可能导致的问题

- 死锁：在wait操作上互为条件的进程陷入无限等待状态
- 饿死：某进程永远不会从阻塞队列中移除



### 有限缓冲区问题

- 只要缓冲区不满，生产者就可以往缓冲区送数据
- 只要缓冲区不空，消费者就可以在缓冲区取数据
- 每次只能有一个进程进入缓冲区



### 有限缓冲区问题算法

- 初始：full=0，empty=n，mutex=1

```c++
do {
	//produce an item in nextp
	wait(empty);
	wait(mutex);
	//add nextp to buffer
	signal(mutex);
	signal(full);
} while (1);
```

```c++
do {
	wait(full)
	wait(mutex);
	//remove an item from buffer to nextc
	signal(mutex);
	signal(empty);
	//consume the item in nextc
} while (1);
```



### 读者写者问题

- 多个进程可以同时读数据
- 读数据时不能写，写数据时不能读
- 不允许多个进程同时写数据



### 读者写者问题算法

- 初始：mutex=1，wrt=1，readcount=0

```c++
wait(mutex);
readcount++;
if (readcount == 1) wait(wrt);
signal(mutex);
//reading is performed
wait(mutex);
readcount--;
if (readcount == 0) signal(wrt);
signal(mutex):
```

```c++
wait(wrt);
//writing is performed
signal(wrt);
```



### 哲学家用餐问题

- 五位哲学家坐在圆桌旁，五根筷子放在桌子上
- 只有当哲学家同时拿起左右手边的筷子，哲学家才可以用餐
- 当五位哲学家都拿起一只筷子时会发生死锁



### 哲学家用餐问题算法

- 一个餐具一个二元信号量，会产生死锁
- 使用范围为-1~4的信号量，最多四位哲学家能取筷子
- 将取两根筷子的操作放入互斥区中，即不能分开取筷子
- 当哲学家手上没有筷子时，奇数哲学家先取左手边的筷子，偶数哲学家先取右手边的筷子



### 管程的原理和优势

- 信号量的控制信息分布在整个程序中，其正确性分析很困难
- 信号量适用不当会导致进程死锁
- 管程定义了一个**数据结构**和能被并发进程执行的一组**操作**，这组操作能改变管程中的数据，实现进程的同步
- 在任何时候，只能有一个进程在管程中执行，调用管程的任何其他进程都被挂起，等待管程可用
- 管程允许进程**等待条件变量**变为真后，进入临界区；管程允许在条件变量设置为真时**发出**信号
