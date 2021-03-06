你
## SQL数据库
1. 连接相关
    1. Inner join 内连接产生的结果是AB的交集
    2. Left [Outer] join 产生表A的全集,而B表中匹配的则有值,则以null值取代
    3. right [Outer] join 产品的b的全集,而a表中没有的值则也null值取代
    4. Full  [Outer] join 产生的是A与b的并集,对于没有匹配的记录,则会以null作为值
    5. select top 子句用于规定要返回的记录的数目
    6. select union操作符用于合并两个或者多个SELECT 语句的结果集( UNION内部的Select 语句必须拥有相同数量的列,列也必须拥有相似的数据类型,每条Select语句中的列的顺序必须相同)
    7. Select
2. 索引(应该建立的字段)
    1. 作为经常需要查询条件的字段
    2. 外键
    3. 经常需要排序的字段
    4. 分组排序的字段


3. mysql中的b-tree索引
    1. b-tree的b是balance
    2. 作用显著减少定位记录所经历的中间过程.
    3. b+tree是B-tree索引的一个变种,大名鼎鼎的MySQL就普遍使用B-tree实现其索引

4. mysql中的hash索引
    1. hash索引其检索效率非常高,索引的检索可以一次定位,所以hash索引的检索效率远高于B-tree索引
    2. Hash索引仅仅满足"=","IN"和"<=>"查询,不能使用范围查询,由于hash索引比较的是进行hash之后的Hash值,所以他只能用于等值的过滤,不能用于基于范围的过滤,因为经过相应的Hash算法处理之后的Hash值的大小关系,并不能保证和Hash运算前完全一样.
    3. hash 索引无法被用来避免数据的排序操作
    4. Hash索引不能利用部分索引键查询(由于has计算的时是利用组合索引一起计算Hash值)
    5. Hash索引在任何时候都不能避免扫描表
        1. 计算原理是将hash键通过hash运算完毕之后将运算结果的Hash值和所对应的行指针信息存放于一个Hash表中,由于不同索引键存在相同的Hash值,所以即使取满足某个Hash键值的数据的记录条数,也无法从hash索引中直接完成查询,还是要通过访问表中的实际数据进行相应的比较,并的到相应的结果
    6. Hash 索引遇到大量Hash值相等的情况后性能并不一定会比B-tree索引性能高
        1. 对于选择性比较低的索引键,如果创建Hash索引,那么将会存在大量记录指针信息存储同一个Hash值相关联,这样要定某一条记录就会非常麻烦,会浪费多次表数据的访问,而造成整体性能的地下

5. mysql的存储引擎介绍与说明
    1. myisam
      1. 默认采用的存储引擎
      2. 不支持事务
      3. 只支持锁定整个表,读锁和写锁是固定的
        4. 不适合有大量查询和修改并存的情况,查询会导致进程有长时间阻塞
        5. 由于MyIsam是锁表,所以某项读写操作比较好使会是的其他进程饿死
      4. 支持全文索引
        5. 但是不支持分词,必须使用者分词后加入空格在写到数据表里,而且少于4个汉子的词会和停用此一样被忽略掉.
      5. 支持GIS数据
        6. 支持空间数据对象:Point.line.polygon,sufrace
    2. innodb
      1. 支持事务处理--其默认的autocommit是默认打开的,每条sql语句都会被封装成一个事务提交,
      2. 外键
      3. 行级锁
      4. 主键范围是myisam的两倍
      5. 不支持全文索引
        6. 是指对char/varchar和text中的每个此建立倒排索引,

6. sql的注入
    1. 把sql命令插入到Web表单提交或者输入域名活页面请求的查询字符串,最终达到欺骗服务器执行恶意SQL的命令
    2. 防止办法
        1. 永远不要信任任何用户的输入
        2. 永远不要使用动态拼装的SQL.
        3. 永远不要使用管理员权限的数据库连接
        4. 不要吧机密信息直接存放,加密或者hash掉密码和敏感信息
        5. 应用的异常信息应该给出尽可能少的提示,最好使用自定义的错误信息对原始错误数据进行包赚
        6. 采用辅助软件或者平台来检测
7. mysql中的锁
  1. 锁是计算机协调多个进程或纯线程并发访问某一个资源的机制.
  2. 在数据库中,出传统的计算资源(CPU/RAM/I/O)的争用以外,数据也是一种供多用户共享的资源
  3. 如何保证数据并发访问的一致性,有效性是所在有数据库必须解决的一个问题,锁冲突也是影响数据库并发访问性能的一个重要因素
  4. 锁对数据库显得尤其重要也更加复杂
  5. 不同的存储引擎支持不同的锁机制
    6. 表级锁
    7. 行级锁
    8. 页面锁:开销和加锁时间界与表锁和行锁之间,会出现思索,锁定力度界于表锁和行锁之间,并发度一般
8. 事务
    1. 读取未提交内容(Read Uncommitted)
    2. 读取提交内从(Read Committed)
    3. Repeatable Read(可重度)
      4. InnoDB和Faicon存储引擎通过多版本控制MVCC,multiversion Concurrency Control)机制解决该问题.
    4. 可串行化(Serializable)
      5. 强制事务排序,使之不可能相互重复,从来解决幻读问题
      6. 在每个度的数据行上加上共享锁.在这个级别上,可能导致大量的超时现象和锁竞争



##　缓存(cache)
1. cache如何实现
  2. 需要遵循的原则(自上而下换出的数据单位越来越笑傲)
    3. L1页面cache缓存渲染后的页面
    4. L2数据cache缓存页面数据
    5. 数据结构cache:缓存响应的数据实体
  3. 保证数据有限时间一致性以及最终一致性
    4. 数据的更新应该是在可预知的时间内自动更新cacha
    5. 保证db中的数据是正确读取的,在cache出现问题的时候可以通过一定手段使得cache与db保持一致
    6. 读写cache应该在一个函数中进行
      7. getfromcache
      8. getfromdb
      9. updateCache
  4. 应该没有全局的cache开关
    5. 每一级cache都应该设置有开关,这是可测性及追查问题简单性的需求
  3. 旁路cache程序可以在未中cache时,选择读db
  4. 穿透性cache:又叫全cache,db值是作为数据恢复的备份.所有读写操作都是通过cache,额外的处理都是cache与db中间的事情
  2. 算法设计
    3. FIFO(很少使用)
      4. 新访问的数据插入到FIFO队列尾部,数据在FIFO队列中顺序移动
      5. 淘汰FIFO队列头部的数据.
    4. LRU(least recently used,最近最少使用)
      5. 最近访问记录来淘汰数据
      6. 如果数据最近被访问过,将来被访问的几率较高,则使用链表保存缓存的数据
        7. 新数据插入到链表头部
        8. 每当缓存数据命中,则将数据移动链表头部;
        9. 当链表满的时候,将链表尾的数据丢弃
    5. LFU(least Frequently used,最不经常使用)
      6. 根据数据的历史访问频率来淘汰数据.如果数据过去访问次数越多,将来被访问的几率限购是比较高
      7. LFU每个数据都有一个引用计数,所有数据块按照引用计数器排序,具有相同引用计数的数据块则按照时间排序
      8. 新加入的数据插入到队列尾部
      8. 队列中的数据被访问之后,引用计数器增加,队列重新排序
      9. 当需要淘汰数据时,将已经排序的列表最后的数据块删除
    6. 评价算法好坏的标准主要有两个:
      7. 命中率要高
      8. 算法容易实现
    7. 设计简单的Cache
      8. 双向链表连接Cache中的数据项,
        9. cache中块的命中可能是随机的.和load进来的数据无关
        10. 双向链表插入/删除很快,可以灵活的调整互相之间的次序,时间复杂度唯一.
      9. 保证链表维持数据项从最近访问到最旧访问的顺序
      10. 每次数据被查到时,都将此数据移动到链表头部
        11. 通过Hash表来提高查询效率
      11. 总结
        12. 对于Cache的每个数据块,我们设计一个数据结构来存储Cache块的内容,并实现一个双向链表,其中属性next和pre时双向链表的两个指针,key用于存储对象的键值,value用户存储要cache块对象本身.
      12. Cache接口
        13. 查询:
          14. 根据键值查询Hashmap,命中则返回节点,否则则返回null
          15. 从双向链表中删除命中的节点,将其重新插入到表头
          16. 所有操作的时间复杂度为O(1)
        14. 插入
          15. 将新的节点关联到hashmap
          16. 如果cache慢了,删除双向链表的尾节点,同时删除Hashmap对应的技术
          17. 将新节点插入到双向链表中头部
        15. 更新与查询类似
        16. 删除
          17. 从双向链表和HashMap中同时删除对应的记录
        17. 参考实现
          18. Apache jcs
2. redis
    1. 数据结构
    <!-- 2. 持久 -->
    3. 复制
    4. cas
    5. 单线程
3. Memcache
4. Tair

## 消息队列
1. JMS  
    1. Queue
    2. Topic
2. kafka
    1. 持久
    2. 复制
    3. Partition
    4. Stream
3. RocketMQ
4. RabbitMQ
5. ActiveMQ

## 数据结构相关
1. hash表
    1. hash表也成散列表,也有直接译做哈希表,他同数组链表以及二叉排序树相比较有明显的区别,
    2. 他能够快速定位到想要查找到的记录,而不是与表中存在的记录的关键字进行比较来查找.
    3. 这源于,他采用了函数映射的思想将记录的储存位置与记录的关键字关联起来了,从而能够快速的进行查找
    4. 时间复杂度变为O(1)
    5. 通过key直接过去到该记录在表中的存储位置,能够省掉中间关键词比较的这个中间环节
2. 原理
    1. 映射函数成为hash函数f: key --> address ,将关键字映射到该记录在表中的存储位置,从而在想要查找到该记录时,可以直接根据关键字和映射关系计算出该记录在表中的存储位置.通常这个映射关系成作为Hash函数.
    2. Hash函数的设计好坏直接影响到了Hash表的操作效率
        1. 考虑关键字的分布特点来设计函数,使的Hash地址均匀分布在整个地址空间当中.
            1. 直接定址法
            2. 平方取中法
            3. 折叠法
            4. 除留取余法
        2. 冲突的解决
            1. 开放地址法
            2. 链地址法
        3. Hash表大小的确定
            1. 根据最终记录存储个数和关键词分布的特点来确定hash表的大小
            2. 动态维护Hash表的容量,溶蚀需要重新计算Hash地址
        4. hash表的平均查找长度
            1. 查找成功的平均查找长度=表中每个元素查成功时的比较次数之和/表中的元素数
            2.
        5. Hash表优缺点
            1. 有点显而易见,能够在常数级的时间复杂度上进行查找,并且插入数据和删除数据都比较容易.
            2. 缺点:比如不支持排序,一样比用线性表要存储更多的空间,并且记录的关键子不能重复.
2. hashMap和hashTable的区别
    1. HashMap和HashTable都实现了Map接口,但决定用哪个之前先要弄清楚他们之间的分别.
    2. HashMap几乎可以等价于HashTable,除了HashMap是非cynchronized,并可以接受null,
    3. HashTable是线程安全的,,是synchronized,多个线程可以共享HashTable;如果没有正常同步的话多线线程是不能共享HashMap的.
    4. HashMap的迭代器(Iterator)是fail-fast迭代器
    5. 单线程环境下HashTable要比HashMap要慢
    6. HashMap不能保证随着时间的推移Map中的元素次序是不变的.
3. 重要术语
    1. sychronized意味着在一次仅有一个线程能够更改HashTable.就是说任何线程要修改更新Hashtable时要活得同步锁,其他线程要等同步锁释放之后才能在次更新HashTable
    2. 结构上的更改是只删除或者插入一个元素,这样会影响map的结构
    3. Fail-safe和iterator迭代器相关.如果某个集合对象创建了Iterator或者ListIterator,然后其他的线程试图"结构上"更改集合对象,将会抛出"ConcurrentModificationException".但是其他线程可以通过set()方法更改集合对象是允许的,因为这并没有从结构上更改集合对象.
4. 能够让HashMap同步
    1. HashMap可以通过下面的术语同步:
        1. Map m = Collection.synchronizeMap(hashMap);
5. Haashtable和HashMap有一个主要的不同:
    1. 线程安全以及速度.
    2. 仅在需要线程安全的时候使用hashTable,
    3. 在高版本上的话请使用ConcurrentHashMap

## 算法相关

## java中堆内存和栈内存详解
1. 栈内存  
    1. 一些基本类型的变量和对象的引用变量都是在含住的栈内存中分配
    2. 堆内存用于存放由new创建的对象和数组
2. java 中的内存分配策略
    1. 静态分配-编译时期就能确定每个数据目标在运行时刻存储空间的需求
    2. 栈式存储分配--动态存储分配
3. 搜索
    1. 二分搜索
4. 排序
    1. 选择排序
        1. 从所有序列中先找到最小的,然后放到第一个位置,
        2. 之后再看剩余元素中最小的,放到第二个位置...
        3. 时间复杂度为N*N
    2. 冒泡排序
        1. 两两比较,将两者较少的升上去
        2. 时间复杂度是(N*N)
        3. 可以通过标记变量来进行优化,优化之后的时间复杂度为n
    3. 插入排序
        1. 性能比冒泡与选择排序都要高
        2. 将一组无序数分成两个去,一个为有序区,一个是无序区
        3. 从无序区域中每次抽取一个数插入有序中的合适的位置,直至所有数全部有序
        4. 这种排序的思路就是一种增量排序
          5. 向一个已有序列的数列中添加元素,并且要保证添加元素后的数列仍然保持其原来的有序性
          6. 可见插入排序算法在添加元素的时候,最多只需要遍历一次即可(N)
    4. 快速排序
        1. 解决一般问题的最佳排序算法,比较适合解决大规模数据的排序.
        2. 需要选择一个准数,通过基准数将大于它和小于它的数无序的放在基准书的两边
        3. 然后通过递归,用上述同样的方法选取一个基准书进行分割,直至整个数列被分割的各部分已经不再被分割
    5. 归并排序
        1. 先递归分解数列,在合并数列就完成了归并排序
    6. 堆排序
        1. 一般讲二叉堆简称为堆:一个顺序存储的完全二叉树
        2. 二叉堆是完全二叉树或者近似完全二叉树
        3. 二叉堆满足两个特性(二叉堆的定义)
          1. 父节点的键值总是大于或等于(小于或等于)任何子节点的键值
          2. 每个节点的左子树和右子树都是一个二叉树(都是最大堆或者最小堆)
          3. 当父节点的键值总是大于或等于任何一个子节点的键值时成为最大堆,
          4. 当父节点的键值总是小于或者等于任何一个子节点的键值时候为最小堆
        4. 一般都用数组来表示堆,i节点的父节点下表就是(i-1)/2.他的左右子节点下表分别是2i+1/2i+2
          1. 首先按照堆的定义将数组R[0...n]调整为堆.称为初始堆,调整R0和Rn
          2. 然后,将R[0...n-1]调整为堆,交换R[0]和R[n-1]
          3. 如此反复,直到交换R[0]和R[1]为止
          4. ![堆排序](http://images2015.cnblogs.com/blog/318837/201604/318837-20160422104524038-1723180638.png)
    7. 桶排序
          1. 最稳定的
          2. 常见排序里面最快的一种,比快排还要快
          3. 桶排序非常快,但是也非常耗空间
          4. ![桶排序](http://images.cnblogs.com/cnblogs_com/kkun/201111/201111251834158044.png)
    8. 基数排序(Radix sort)
          1. 原理类似桶排序,这里总是需要10个桶,且多次使用
          2. 第一次以个位数字排序
          3. 第二次以十位数字排序
          4. 一次类推
5. 高级算法
    1. 贪婪算法
      1. 时刻保证局部最优,当前看来是最优选择
      2. 没有固定算法框架,算法设计关键是贪心策略的选择
      3. 必须几倍无有效性,某个状态以后的过程不会影响以前的状态,只与当前状态有关
    2. 回溯算法
      1. 回溯算法也叫试探发,它是一种系统地搜索问题的解的方法.
      2. 回溯法的基本思想是:从一条路往前走,能进则进不能进则退回来,换一条路再试
      3. 一般步骤:
        1. 定义一个解空间,他包含问题的解
        2. 利用适于搜索的方法组织解空间
        3. 利用深度优先法搜索解空间;
        4. 利用限界函数避免移动到不可能产生解的子空间
    3. 剪枝算法
        1. 剪枝策略,属于算法优化范畴,通常应用在DFS和BFS搜索算法中,剪枝策略就是寻找过滤条件,提前减少不必要的搜索路径
        2. 应用剪枝优化的核心问题是设计剪枝判断方法,即确定哪些枝条应当舍弃,哪些枝条应当保留的方法
        3. 三原则:曾雀/准确/高效
    4. 动态规划
6. 大数据算法
    1. hash分桶
    2. 统计

## 分布式
1. 负载均衡
    1. nginx
    2. fegin(spring-cloud)
    3. XX
    4. 当然还有基于硬件的负载均衡
2. 水平伸缩
3. 集群
4. 分片
    1. key-hash
    2. 一致性hash
5. 异步
6. 消峰
7. 分库分表
8. 锁
    1. 悲观锁
    2. 乐观锁
    3. 行级锁
    4. 分布式锁
    5. 分区排队
9. 分布式事务(HAWQ)
## 反射
1. 对于一个任意的类都能知道其所有属性和方法
2. 对于一个任意的对象都能够通过反射机制来调用一个类的任意方法.
3. 这种动态获取类信息及动态调用类对象的方法的功能称为java的反射机制.
4. 反射的作用
    1. 动态的创建类的实例,将类绑定到现有的对象中,或从现有的对象中获取类型
    2. 应用程序需要在运行时从某个特定的程序集中载入一个车特定的类
## 多媒体相关
1. bit-map
2. 语音
    1. 数据采集--数据预处理-数据标注(标注系统)--数据训练--数据上线等
3. 视频
    1. ffmpeg
    2. openCV

##  MongoDB

## HBase
1. flash数据结构




##　java语言
1. 异常
2. 类继承
3. 泛型
4. 内部类
5. 反射
6. 序列化
7. 对象类
8. 字符串类
9. 引用
    1. 弱引用:弱引用的对象有更短暂的生命周期.
    2. 强引用:使用最普遍的一种引用.如果一个对象具有强引用,那垃圾回收期绝对不会回收他.
    3. 软引用:如果一个对象具有软引用,则内存空间足够,垃圾回收期就不会回收他,当内存空间不足,就会回收这些对象的内存
    4. 幻影引用:形同虚设,与其他几种引用都不同.虚引用不会决定对象的生命周期.跟踪对象被垃圾回收的活动.
10. 类库
    1. 集合
    2. 流
11. java中object常用方法
    1. clone
    2. equals
    3. finalize
    4. getclass
    5. hashcode
    6. notify
    7. notifyAll
    8. toString
12. java中对多态的理解
    1. 所谓多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定而是在程序运行期间才确定,即一个引用变量到底会指向那个类的实例对象改引用变量发出的方法调用到底是哪个类中的实现方法必须在由程序运行期间才能确定.
    2. 因为在程序运行时才确定具体的类,这样不用修改源程序代码,就可以让引用变量绑定到不同的类的实现上了.从而导致该引用调用的具体方法随之改变,即不修改程序代码就可以改变程序运行时所绑定的具体代码,让程序可以选择多个运行状态,这样就是多态性.
13. java8的内存分代改进
    1. java8中的持久代码已经被彻底删除了取而代之的是另一个内存区域也被称之为元空间
    2. 元空间
        1. 充分利用了Java语言规范中的好处,类以及先关元数据的生命中期与类加载器一直
        2. 内存分配模型
        3. 调优

14. comparable和comparator的区别
    1. comparable可以认为是一个内比较器
    2. comparator可以认为是一个外比较器
##　linux相关

## 云计算
1. IAAS Openstack
2. 虚拟化
3. 容器技术
    1. docker
    2. k8s
## 分布式存储相关
1. NFS/阵列存储集中式存储
2. 龙存
3. ceph的分部式存储

## 脚本语言python/shell
1. puppet
2. chef自动化运维
3. zebix

## 深度神经网络相关
1. tensor flow
2. dnn4j
3. GPU与并行计算

## 多线程与并发
1. ![线程状态](http://img.blog.csdn.net/20170304134110520?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc2luYXRfMzU1MTIyNDU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
2. 线程状态
    1. 新建:新建一个线程对象
    2. 可运行:从锁池里难道锁标识
    3. 运行:OS选中执行
    4. 阻塞:等待用户输入
        1. 线程因为某种原因放弃了cpu使用权,也让出了cpu timelice ,暂时停止运行.直到线程进入可运行(runnable)状态,才有机会再次获得cpu timeslice转到(running)状态.
        2. 分类
            1. 等待阻塞:运行的线程执行o.wait()方法,JVM会把该线程放入等待队列(waitting queue)中
            2. 同步阻塞:运行的线程在获取对象的同步锁时,若该同步锁被别的线程占用,JVM则会把改现车给你放入锁池中(lock pool)中.
            3. 其他阻塞:运行的线程执行Thread.sleep(long ms)或t.join()方法,或者发出了I/O请求时,JVM会把改线程置为阻塞状态.当sleep()状态超时/join()等待线程终止是hi欧,或者I/o处理完毕的时候,线程重新转入可以运行(runnable)状态.   
    5. 死亡:run()/main()方法结束/或者异常退出

2. 线程间通信模型
    1. 线程间的相互作用:线程之间需要一些协调通信,来共同完成一件任务.
    2. wait()方法:使得当前线程必须要等待,等到另外一个线程调用notify()或者notifyAll()
        1. 线程释放他对锁的拥有权,然后等待另外的线程来通知它,这样他才能重新获得所的拥有权和恢复执行
        2. wait()方法必须放在synchronized方法或synchronized块中.
        3. 另一个会导致现车给你暂停的方法:Thread.sleep(),他会导致线程睡眠指定的毫秒数,但是现车给你在睡眠过程中不会释放掉对象的锁的.
    3. notify()方法
        1. notify()方法会唤醒一个等待当前对象锁的线程.
            1. 如果多个线程在等待,他们中的一个将会选择被唤醒.这种选择是随意的,和具体实现有关.
            2. 被唤醒的线程是不能被执行的,需要等到当前线程放弃这个对象的锁.
            3. 被唤醒的线程将和其他线程以通常的方式进行竞争,来获得对象的锁.
##　java内存模型的理解以及其在并发当中的作用
1. Java平台自动集成了线程阿姨级多处理器技术,这种机车姑奶程度比java以前诞生的计算机语言要厉害的多.改语言针对多种易购屁股虐爱的平台独立性而使得多线程技术知己也是具有开拓性的一面.
2. java内存模型,内存模型描述了程序中各个变量(实例域/静态域和数组元素)之间的关系,以及在实际计算机系统中将变量存储到内存和从内存中取出变量的这样的细节.对象最终是要存储在内存里面的,这点没有错,但是编译器/运行库/处理器或者系统缓存可以有特权在变量指定内存内存位置存储或者取出变量的值
3. JMM:java内存模型允许编译器和缓存一数据在处理器特定的缓存(或寄存器)和主存之间移动的次序有重要的特权,除非程序员使用了final或者synchronized明确请求了某些可见性的保证.
4. 在java 中应为不同的目的可以将java划分为两种内存模型
    1. GC内存模型
    2. 并发内存模型
5. GC内存模型
    1. java与C++ 之间有一堵内存动态分配与垃圾收集技术所围成的"高墙",墙外面的人想进去,墙里面的人想出来.java在执行java程序的过程中会把他管理的内存划分若干不同功能的数据管理区域.
    2. ![java-gc内存模型](http://images2015.cnblogs.com/blog/286989/201611/286989-20161124093427206-761806286.jpg)
    3. ![java-堆内存模型](http://images2015.cnblogs.com/blog/286989/201701/286989-20170112150556791-818433561.png)
    4. ![分代的堆空间](http://images2015.cnblogs.com/blog/286989/201701/286989-20170112150607822-1924598543.jpg)
6. hotspot中的gc内存模型
    1. 栈/虚拟机/本地方法栈
        1. 在栈中会给每个线程创建一个站.线程越多,栈的内存空间使用越大.
        2. 对于每个线程栈,当一个方法在线程中执行的时候,会在现车给你栈中创建一个栈帧(stack frame),用于存放该方法的上下文局部变量表/操作数栈/方法返回值的地址.每一个方法调用到执行完毕的过程,就是对应一个栈帧入栈到出栈的过程.
        3. 本地方法栈为虚拟机执行native方法服务的,虚拟机栈为虚拟机执行java(字节码)服务的.
    2. 堆/方法区
        1. 方法区就是在堆中称为永久代的堆区域.
        2. 几乎所有的对象/数组的内存空间都在堆上.
        3. 虚拟机对分为
            1. 新生代
                1. 一个eden区
                2. 两个survivor区
                3. 策略:mark-copy
            2. 老年代
                1. 新生代中的对象经过若干轮回gc后还存活/或survisor在gc内存不够的时候.会把当前对象移动到老年代.
                2. 策略:mark-compact
            3. 永久代
                1. 永久代不参与gc.
                2. 代码/常量数据/类信息
                3. 清除的是类信息以及常量池
    3. 程序技术器
        程序计数器用于技术某个线程下执行指令的位置,程序计数器也是线程私有的.
7. 并发内存模型
    1. Java视图定义一个Java内存模型(Java memory model jmm)来屏蔽掉各种硬件/操作系统的内存访问差异,以实现java程序在各个平台下都能达到一致的内存访问效果.
    2. java内存模型主要目标是定义程序中各个变量的访问规则,即在虚拟机中将变量存储到内存中和从内存中取出变量这样的底层细节.
    3. ![Java中并发内存模型](http://images2015.cnblogs.com/blog/286989/201611/286989-20161124093612487-2048767455.jpg)
    4. java内存模型中规定了所有变量都存储到主存储中.每一个线程都有一个自己的工作内存(如cpu中的告诉缓存).线程中的工作内存保存了该线程使用到的变量的主内存的副本拷贝.线程对变量的所有操作(读取/赋值)必须在改线程的工作内存中进行,不同线程之间无法直接访问对方工作内存变量.线程之间的变量的值传递需要通过主内存来完成.
    5. 操作解释(变量从主内存copy到工作内存,以及从工作内存同步到主内存)
        1. lock(锁定):用于主内存,他吧一个车变量标记为一条现车给你独占状态
        2. unlock(解锁)
        3. read(读取)
        4. load(载入)
        5. use(使用)
        6. assign(赋值)
        7. store(存储)
        8. write(写入)
    6. volatile型变量的特殊规则
        1. 保证此变量对所有线程的可见性
            1. 普通变值在线程之间传递需要通过主内存来完成
        2. 禁止指令重排序优化
8. 原子性、可见性、有序性
    1. 原子性（Atomicity）：
        1. 由java 内存模型来直接保证个呢原子性变量包括read、load、assing、use、stroe和write。我们大致认为基本数据类型的访问读写都是具备原子性的。
        2. java 内存模型还提供了lock和unlock来满足更大方位的原子性需求
        3. 更高层次的字节码指令monitorenter和monitorexit来隐式的使用这些操作，这两个指令反应到java代码中就是同步代码块--synchronized关键字，因此在synchronized块之间的操作也具备原子性
    2. 可见性
        1. 一个线程修改共享变量，其他现车给你能够立即得知这个修改
    3. 有序性
## GC
1. GC收集器类型
    1. 串行
    2. CMS
    3. 并行
    4. G1
2. 算法
    1. 复制
    2. 标记清理
    3. 标记整理
## IO/NIO
    1. 同步阻塞
    2. 同步非阻塞
    3. 基于信号
    4. 多路复用
    5. 异步IO
##　类加载器
1. 双亲委派
2. OSGI

## 性能优化
1. 分层优化
    1. 系统级别
    2. 中间件级别
    3. JVM级别
    4. 代码级别
2. 分段优化
    1. 前端
    2. 后端
    3. 资源

## 测试框架
1. 老框架
    1. junit
    2. easymock
2. 新框架
    1. testing
    2. mockito

## 日志框架
1. 老框架
    1. common logging
    2. log4j,
    3. jdk logger
2. 新框架
    1. slf4j
    2. logback

##　设计模式
1. 门面
2. 代理
3. 工厂
4. 单例


## 网络
1. TCP/IP
2. HTTP


##工作业绩
1. 技术攻关
2. 应急
3. 创新
4. 分享
5. 项目管理
6. 程序开发案例
7. 项目设计案例

## 非技术
1. 责任心
2. 团队精神
3. 主动性
4. 性格
5. 年龄
6. 期待
    1. 做完语音行业应用
    2. 探索更多智能语音交互相关应用
    3. 将语音技术在产品端能有更多的创新
    4. 在语音技术的框架端/应用开发库端更加的好用,能让更多人用,且用起来更简单方便
7. 职业规划
