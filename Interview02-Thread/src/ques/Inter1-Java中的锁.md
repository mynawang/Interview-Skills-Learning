# Inter1-Java中的锁

http://ifeve.com/java_lock_see/
http://www.importnew.com/21933.html

1.自旋锁
----------------
自旋锁可以使线程在没有取得锁的时候，不被挂起，而转去执行一个空循环，（即所谓的自旋，就是自己执行空循环），若在若干个空循环后，线程如果可以获得锁，则继续执行。若线程依然不能获得锁，才会被挂起。

使用自旋锁后，线程被挂起的几率相对减少，线程执行的连贯性相对加强。因此，对于那些锁竞争不是很激烈，锁占用时间很短的并发线程，具有一定的积极意义，但对于锁竞争激烈，单线程锁占用很长时间的并发程序，自旋锁在自旋等待后，往往毅然无法获得对应的锁，不仅仅白白浪费了CPU时间，最终还是免不了被挂起的操作 ，反而浪费了系统的资源。

在JDK1.6中，Java虚拟机提供-XX:+UseSpinning参数来开启自旋锁，使用-XX:PreBlockSpin参数来设置自旋锁等待的次数。

在JDK1.7开始，自旋锁的参数被取消，虚拟机不再支持由用户配置自旋锁，自旋锁总是会执行，自旋锁次数也由虚拟机自动调整。

2.自旋锁的其他种类
----------------

3.阻塞锁
----------------
阻塞锁，可以说是让线程进入阻塞状态进行等待，当获得相应的信号（唤醒、时间）时，才可以进入线程的准备就绪状态，准备就绪状态的所有线程，通过竞争，进入运行状态。Java中，能够进入\退出、阻塞状态或包含阻塞锁的方法有：synchronized同步锁(其中的重量锁)，ReentrantLock，Object.wait()\notify()，LockSupport.park()\unpart()（j.u.c经常使用）

阻塞锁的优势在于，阻塞的线程不会占用cpu时间， 不会导致 cpu占用率过高，但进入时间以及恢复时间都要比自旋锁略慢。

在竞争激烈的情况下 阻塞锁的性能要明显高于 自旋锁。理想的情况则是：在线程竞争不激烈的情况下，使用自旋锁，竞争激烈的情况下使用，阻塞锁。

4.可重入锁
----------------
《并发编程的艺术》p136

5.读写锁
----------------
《并发编程的艺术》p140

6.互斥锁
----------------

7.悲观锁
----------------
http://www.importnew.com/21037.html

在关系数据库管理系统里，悲观并发控制（又名“悲观锁”，Pessimistic Concurency Control，缩写“PCC”）是一种并发控制的方法。它可以阻止一个事务以影响其他用户的方式来修改数据。如果一个事务执行的操作为某行数据应用了锁，那只有当这个事务把锁释放，其他事务才能执行与该锁冲突的操作。

悲观并发控制主要用于数据争用激烈的环境，以及发生并发冲突时使用锁保护数据的成本要低于回滚事务的成本的环境中。

悲观锁，正如其名，它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度(悲观)，因此，在整个数据处理过程中，将数据处于锁定状态。 悲观锁的实现，往往依靠数据库提供的锁机制 （也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）

8.乐观锁
----------------
在关系数据库管理系统里，乐观并发控制（又名“乐观锁”，Optimistic Concurrenct Control，缩写“occ”）是一种并发控制的方法。它假设多用户并发的事务在处理时不会彼此互相影响，各事务能够在不产生锁的情况下处理各自影响的那部分数据。在提交数据更新之前，每个事务会先检查在该事务读取数据后，有没有其他事务又修改了该数据。如果其他事务有更新的话，在提交的事务会进行回滚。

乐观锁（ Optimistic Locking ） 相对悲观锁而言，乐观锁假设认为数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让返回用户错误的信息，让用户决定如何去做。

相对于悲观锁，在对数据库进行处理的时候，乐观锁并不会使用数据库提供的锁机制。一般的实现乐观锁的方式就是记录数据版本。实现数据版本有两种方式，第一种是使用版本号，第二种是使用时间戳。

9.公平锁
----------------
某个对象的锁对所有线程都是公平的，先到先得。每次加锁前都会检查队列里面有没有排队等待的线程，没有才会尝试获取锁。

10.非公平锁
----------------
当一个线程采用非公平锁这种方式获取锁时，该线程会首先去尝试获取锁而不是等待。如果没有获取成功，那么它才会去队列里面等待。

11.偏向锁
----------------

12.对象锁

13.线程锁

14.锁粗化

15.轻量级锁

16.锁消除

17.锁膨胀

18.信号量



1、自旋锁 ,自旋，jvm默认是10次吧，有jvm自己控制。for去争取锁
2、阻塞锁 被阻塞的线程，不会争夺锁。
3、可重入锁 多次进入改锁的域
4、读写锁
5、互斥锁 锁本身就是互斥的
6、悲观锁 不相信，这里会是安全的，必须全部上锁
7、乐观锁 相信，这里是安全的。
8、公平锁 有优先级的锁
9、非公平锁 无优先级的锁
10、偏向锁 无竞争不锁，有竞争挂起，转为轻量锁
11、对象锁 锁住对象
12、线程锁
13、锁粗化 多锁变成一个，自己处理
14、轻量级锁 CAS 实现
15、锁消除 偏向锁就是锁消除的一种
16、锁膨胀 jvm实现，锁粗化
17、信号量 使用阻塞锁 实现的一种策略
