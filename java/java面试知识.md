###JAVA基础知识
####Throwable Java异常处理 
Java异常都继承自Throwable接口，可分为Error(如OOM等)和Exception，其中Exception又包括RuntimeException(unchecked exception)和除RuntimeException的异常(checked exception)，常见的RuntimeException如下：  
NullPointerException  
ArrayIndexOutOfBoundsException  
IllegalArgumentException  
IllegalStateException  

通常由用户try...catch来捕获checkedException. RuntimeException也称为unchecked Exception即非检查异常，程序中应该减少对该异常的捕获，出现该异常应该定位程序出问题的原因。其他的Exception也称为checked Exception如果没有捕获会导致编译不通过等。
####流
字节流：InputStream和OutputStream  
字符流：Reader和Writer 
####容器 
**HashMap和HashTable的区别**   
1.继承不同HashMap继承AbstractMap而HashTable继承Dictionary。  
2.HashTable是线程安全的，而HashMap不是线程安全的  
3.Hashtable中key和value都不允许为null，而HashMap可以允许key或者value为null  
4.两个遍历方式的内部实现上不同。都可以使用Iterator，但历史原因Hashtable还可以使用Enumeration方式  
5.HashTable直接使用对象的hashCode，而HashMap需要重新计算hash值  
6.HashMap的初始容量为16，而HashTable为11，且HashMap的扩容方式是每次增长2倍，而HashTable每次增长2倍+1  
**HashMap和ConcurrentHashMap的区别**  
1.ConcurrentHashMap是线程安全的，而HashMap是线程不安全的。  
2.ConcurrentHashMap不是使用synchronized来保证同步的，而是对map进行分段，在插入时只使用分段锁锁定特定的段。  
3.ConcurrentHashMap由许多个Segment组成，每个段是一个小的hashtable   
4.ConcurrentHashMap的读操作不加锁，写需要加锁    

**Vector和List的区别**  
1、Vector是线程安全的，而List不是线程安全的  
2、Vector通过Enumeration 的方式进行遍历，如下所示：  
Enumeration enu=vec.elements();  
     while (enu.hasMoreElements()) {  
        Object object = (Object) enu.nextElement();  
         System.out.println(object);        
    }   
 
**Java容器继承关系**  
Collection(root)  
List  
--LinkedList  
--ArrayList  
--Vector（线程安全）  
--Stack （线程安全）  
Set  
--HashSet  
--TreeSet  

不属于Collection子类  
Map   
--HashMap  
--WeakHashMap  
--TreeMap  

Dictionary  
--HashTable(线程安全)

####JVM 
**JVM的结构**   
主要包括有程序计数器、Java虚拟机栈、本地方法栈、方法区、堆。其中Java堆和方法区是线程共享的。程序计数器、Java虚拟栈、本地方法栈是线程私有内存区。堆一般抛出的异常为OutofMemoryError,而栈一般抛出的异常为StackOverflowError.  

Heap用来保存Java对象与数组等。Heap可以分为新生代与旧生代，新生代包括Eden、s0、s1区。持久代主要保存class,method,filed等对象  
  
**JVM垃圾回收机制**  
对象的创建会在Eden区中产生，如果Eden区没有足够的空间容纳下创建的对象时，则会触发YGC，YGC的主要过程是将Eden区和From Survivor区的对象拷贝到To Survivor区中，若当To Survivor区的空间不足时，会将对象直接复制到旧生代中。在YGC时会将那些经过多次YGC仍存活的对象拷贝到旧生代中。  

**ClassLoader加载类**  
ClassLoader的功能是在程序执行时，用于加载具体运行的类。ClassLoader采用双亲委托模式来加载Class，在JVM运行时会产生三个ClassLoader：Bootstrap ClassLoader、Extension ClassLoader、AppClassLoader，他们之间存在一定的父子关系。具体的类加载过程时：首先委托给自己的parent ClassLoader去加载，若parent加载成功则返回，否则由parent的Parent进行加载。Parent ClassLoader都不能够加载依赖的类，则由当前使用的ClassLoader进行加载该类，若加载不到，则报错。  
备注：AppClassLoader的Parent是Extension ClassLoader；Extension ClassLoader的Parent是Bootstrap ClassLoader
####JAVA反射&动态代理    
**JAVA反射**  
通过反射可以获取到任何一个已知名称Class的内部信息  

**动态代理**  
一般的情况下每个代理类编译之后都会生成一个class文件，代理类所实现的接口和方法都是被固定的，这种代理称为静态代理。而动态代理只指系统在运行时动态创建代理类。实现动态代理的方式存在两种：  
1.采用JDK的动态代理实现机制，JDK实现动态代理包含InvocationHandler接口与Proxy类，采用JDK的方式需要依靠接口实现，如果类没有实现接口，则不能使用JDK代理；  
2.采用CGLIB动态代理实现机制，对指定的目标类生成一个子类，并覆盖其中方法实现增强，因为采用的是继承方式，所以不能对final修饰的类进行代理。    
####Spring 
**IOC**  
概念：通过IOC容器，利用依赖关系注入的方式，实现对象之间的解耦 
  
**AOP**：  
面向切面的编程。AOP中几个重要的概念：切面，关注点的模块化，横切多个对象，用<aop:aspect>配置切面，切面关注具体的行为；连接点，程序执行过程中明确的点，如方法调用等；通知，在特点连接点，AOP框架执行的动作；切入点，一系列连接点的集合；目标对象：包含连接点的对象；AOP代理：AOP框架创建的代理对象。  
####String操作
####比较器 
Comparable与Comparator的区别：都是用来实现集合元素的比较、排序的。Comparable是在集合内部定义的方法实现的排序，而Comparator是在集合外部实现的排序。Comparable用于自比较，Comparator外部程序实现比较。  
####深浅拷贝  
**浅拷贝**：只复制对象，对象内部存在的指向其他对象数组或者引用则不复制。  
**深拷贝**：复制对象且复制对象中的引用。
####多线程&线程池
**创建线程**：线程创建的三种方式：继承Thread类、实现Runnable接口、实现Callable接口。其中实现Runnable与Callable的区别在于后者可以通过Future获取线程执行之后的返回值。  
**线程同步与通信**：通过加锁的方式进行同步。同步锁synchroinzed与Reentrantlock。这两种锁的区别：前者是JVM层面实现，系统在可以监控锁的释放，后者是用代码实现，系统无法自动释放锁，需要在代码中显示释放锁lock.unlock()，Reentrantlock同时具有中断锁等候和定时锁等候等特点。在并发量较小的情况下采用synchronized比较合适，在高并发的情况下需要可以采用ReentrantLock。  
**线程池**  
**线程池使用场景**：  为提高任务的处理效率，我们一般会创建线程来执行特定的任务。然而线程的创建和销毁是一个耗时的操作。如果在程序中频繁的创建和销毁线程，会对程序的反应速度造成严重的影响。此时可以考虑使用线程池代替。  
**线程池创建**
Java里面线程池的顶级接口是Executor，但是严格意义上讲Executor并不是一个线程池，而只是一个执行线程的工具。真正的线程池接口是ExecutorService。  
ExecutorService pool = Executors. newSingleThreadExecutor();  
Thread t1 = new MyThread();  
pool.execute(t1);  

**线程池类型**

1. newSingleThreadExecutor  
创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行.
  
2. newFixedThreadPool  
创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。  

3. newCachedThreadPool  
创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，
那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。 
 
4. newScheduledThreadPool  
创建一个大小无限的线程池。此线程池支持定时以及周期性执行任务的需求。

**线程池处理过程**：当任务提交至线程池时，首先会判断线程池的核心线程是否已满，如果没满则创建新线程执行当前任务，否则判断当前任务队列是否已满，若未满则将任务放入任务队列中，否则判断线程池的线程个数是否超过最大线程数，若超过则采用拒绝策略来处理当前任务，否则创建线程来执行当前任务。  
**线程中断**：线程中断并非停止当前线程的运行，需要程序通过中断检测停止程序的运行。  
####面向对象
####Java中的一些关键字  
static:静态方法、静态变量、静态代码块、静态内部类
final:final变量，基本类型值不能改变，引用类型引用不能改变，final类无法被继承，final方法该方法不能被扩展   
finally：不一定被执行  
finalize：垃圾回收，类可以实现该方法，在GC时该方法被调用。GC释放内存中的垃圾，finalize可以释放文件或者socket连接之类的资源。

####Java中基本类型与包装类型的区别
1、在Java中，一切皆对象，但八大基本类型却不是对象。  
2、声明方式的不同，基本类型无需通过new关键字来创建，而封装类型需new关键字  
3、存储方式及位置的不同，基本类型是直接存储变量的值保存在堆栈中能高效的存取，封装类型需要通过引用指向实例，具体的实例保存在堆中  
4、初始值的不同，封装类型的初始值为null，基本类型的的初始值视具体的类型而定，比如int类型的初始值为0，boolean类型为false；  
5、使用方式的不同，比如与集合类合作使用时只能使用包装类型。

####面向对象的特征
封装  
继承  
多态  
抽象

####Java继承（抽象类）和接口的区别
1、继承只能单继承，接口可以实现多个接口  
2、继承的抽象类允许有实现了的方法，接口中的方法必须都是未实现的  
3、继承中的抽象类中的可以由私有的方法或者变量，抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public。  

而接口中所有的方法和属性都是公开的，接口中的变量会被隐式地指定为public static final变量（并且只能是public static final变量，用private修饰会报编译错误)   
4、抽象类可以实现接口，而接口是不能继承或实现抽象类的  
5、抽象类一般用于继承，是is-a关系，而接口的实现是can-do的关系  

####JAVA 内部类使用方式
[http://www.cnblogs.com/nerxious/archive/2013/01/24/2875649.html  ](http://www.cnblogs.com/nerxious/archive/2013/01/24/2875649.html "JAVA内部类")  
1、内部类可以随意使用外部类的成员变量（包括私有）而不用生成外部类的对象，这也是内部类的唯一优点  
Outer.in object = new Outer().new in();  
或者  
Outer outer = new Outer();  
Outer.in object = outer.new in();  
2、内部类可以是私有的也可以是共有的，若内部类加上static关键字，则访问的外部类的变量也需要加上static关键字。  
3、若内部类通过private 修饰，则main()函数里面无法直接通过Outer.in object = new Outer().new in()的方式进行调用。只能够在外部类中进行访问。  
4、若为静态内部类，则外部可以直接通过Outer.in object = new Outer.in();的方式进行创建  
5、内部类访问的外部变量需要final进行修饰，其原因如下：   
因为生命周期的原因。方法中的局部变量，方法结束后这个变量就要释放掉，final保证这个变量始终指向一个对象。
例子如下： 

    class Outer{   
       public static void main(String[] args){  
            Outer out = new Outer();  
            Object obj = out.method();  
       }   
       Object method(){  
          int locvar = 1;  
          class Inner{  
            void displayLocvar(){  
            System.out.println("locvar = " + locvar);  
            }  
          }  
          Object in = new Inner();  
          return in;   
       }  
    }
当out.method()方法执行结束后，局部变量 locvar 就消失了，但是在method（）方法中 obj in = new Inner() 产生的 in 对象还存在引用obj，这样对象就访问了一个不存在的变量，是不允许的。这种矛盾是由局部内部类可以访问局部变量但是局部内部类对象和局部变量的生命周期不同而引起的。

  
