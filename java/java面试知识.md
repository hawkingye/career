###JAVA基础知识
####Throwable Java异常处理 
Java异常都继承自Throwable接口，可分为Error(如OOM等)和Exception，其中Exception又包括RuntimeException和除RuntimeException的异常，RuntimeException(NullPointer、ArrayIndexOutOfBoundsException等等)也称为unchecked Exception即非检查异常，程序中应该减少对该异常的捕获，出现该异常应该定位程序出问题的原因。其他的Exception也称为checked Exception如果没有捕获会导致编译不通过等。
####流
字节流：InputStream和OutputStream  
字符流：Reader和Writer 
####容器 
**HashMap和HashTable的区别**   
1.继承不同HashMap继承AbstractMap而HashTable继承Dictionary。
2.HashTable是线程安全的，而HashMap不是线程安全的  
3.Hashtable中key和value都不允许为null，而HashMap可以允许只有一个key为null
4.两个遍历方式的内部实现上不同。都可以使用Iterator，但历史原因Hashtable还可以使用Enumeration方式。  
5.HashTable直接使用对象的hashCode，而HashMap需要重新计算hash值  
6.HashMap的初始容量为16，而HashTable为11，且HashMap的扩容方式是每次增长2倍，而HashTable每次增长2倍+1  
**HashMap和ConcurrentHashMap的区别**  
1.ConcurrentHashMap是线程安全的，而HashMap是线程不安全的。  
2.ConcurrentHashMap不是使用synchronized来保证同步的，而是对map进行分段，在插入时只使用分段锁锁定特定的段。  
3.ConcurrentHashMap由许多个Segment组成，每个段是一个小的hashtable   
4.ConcurrentHashMap的读操作不加锁，写需要加锁    
**Java容器继承关系**  
Collection  
List  
 LinkedList  
 ArrayList  
 Vector  
  Stack  
Set  
Map  
 Hashtable  
 HashMap  
 WeakHashMap  
####JVM 
**JVM的结构**   
主要包括有程序计数器、Java虚拟机栈、本地方法栈、方法区、堆。其中Java堆和方法区是线程共享的。Heap用来保存Java对象与数组等。Heap可以分为新生代与旧生代，新生代包括Eden、s0、s1区。持久代主要保存class,method,filed等对象  
**JVM垃圾回收机制**  
对象的创建会在Eden区中产生，如果Eden区没有足够的空间容纳下创建的对象时，则会触发YGC，YGC的主要过程是将Eden区和From Survivor区的对象拷贝到To Survivor区中，若当To Survivor区的空间不足时，会将对象直接复制到旧生代中。在YGC时会将那些经过多次YGC扔存活的对象拷贝到旧生代中。  
**ClassLoader加载类**  
ClassLoader的功能是在程序执行时，用于加载具
体运行的类。ClassLoader采用双亲委托模式来加载Class，在JVM运行时会产生三个ClassLoader：Bootstrap ClassLoader、Extension ClassLoader、AppClassLoader，他们之间存在一定的父子关系。具体的类加载过程时：首先委托给自己的parent ClassLoader去加载，若parent加载成功则返回，否则由parent的Parent进行加载。Parent ClassLoader都不能够加载依赖的类，则由当前使用的ClassLoader进行加载该类，若加载不到，则报错。  
备注：AppClassLoader的Parent是Extension ClassLoader；Extension ClassLoader的Parent是null  
####JAVA反射&动态代理    
**JAVA反射**  
通过反射可以获取到任何一个已经名称Class的内部信息  
**动态代理**  
一般的情况下每个代理类编译之后都会生成一个class文件，代理类所实现的接口和方法都是被固定的，这种代理称为静态代理。而动态代理只指系统在运行时动态创建代理类。实现动态代理的方式存在两种：1.采用JDK的动态代理实现机制，JDK实现动态代理包含InvocationHandler接口与Proxy类，采用JDK的方式需要依靠接口实现，如果类没有实现接口，则不能使用JDK代理；2.采用CGLIB动态代理实现机制，对指定的目标类生成一个子类，并覆盖其中方法实现增强，因为采用的是继承方式，所以不能对final修饰的类进行代理。    
####Spring 
**IOC**  
概念：通过IOC容器，利用依赖关系注入的方式，实现对象之间的解耦   
**AOP**：  
面向切面的编程。AOP中几个重要的概念：切面，关注点的模块化，横切多个对象，用<aop:aspect>配置切面，切面关注具体的行为；连接点，程序执行过程中明确的点，如方法调用等；通知，在特点连接点，AOP框架执行的动作；切入点，一个通知将被英法的一系列连接点的集合；目标对象：包含连接点的对象；AOP代理：AOP框架创建的代理对象。  
####String操作
####比较器 
Comparable与Comparator的区别：都是用来实现集合元素的比较、排序的。Comparable是在集合内部定义的方法实现的排序，而Comparator是在集合外部实现的排序。Comparable用于自比较，Comparator外部程序实现比较。  
####深浅拷贝  
**浅拷贝**：只复制对象，对象内部存在的志向其他对象数组或者引用则不复制。  
**深拷贝**：复制对象且复制对象中的引用。
####多线程&线程池
**创建线程**：线程创建的三种方式：继承Thread类、实现Runnable接口、实现Callable接口。其中实现Runnable与Callable的区别在于后者可以通过Future获取线程执行之后的返回值。  
**线程同步与通信**：通过加锁的方式进行同步。同步锁synchroinzed与Reentrantlock。这两种锁的区别：前者是JVM层面实现，系统在可以监控锁的释放，后者是用代码实现，系统无法自动释放锁，需要在代码中显示释放锁lock.unlock()，Reentrantlock同时具有中断锁等候和定时锁等候等特点。在并发量较小的情况下采用synchronized比较合适，在高并发的情况下需要可以采用ReentrantLock。  
**线程池**  
**线程池使用场景**：  为提高任务的处理效率，我们一般会创建线程来执行特定的任务。然而线程的创建和销毁是一个耗时的操作。如果在程序中频繁的创建和销毁线程，会对程序的反应速度造成炎症的影响。此次可以考虑使用线程池代替。  
**线程池处理过程**：当任务提交至线程池时，首先会判断线程池的核心线程是否已满，如果没满则创建新线程执行当前任务，否则判断当前任务队列是否已满，若未满则将任务放入任务队列中，否则判断线程池的线程个数是否超过最大线程数，若超过则采用拒绝策略来处理当前任务，否则创建线程来执行当前任务。  
**线程中断**：线程中断并非停止当前线程的运行，需要程序通过中断检测停止程序的运行。  
####面向对象
####Java中的一些关键字  
static:静态方法、静态变量、静态代码块、静态内部类
final:final变量，基本类型值不能改变，引用类型引用不能改变，final类无法被继承，final方法该方法不能被扩展   
finally：不一定被执行  
finalize：垃圾回收，类可以实现该方法，在GC时该方法被调用。GC释放内存中的垃圾，finalize可以释放文件或者socket连接之类的资源。
  
