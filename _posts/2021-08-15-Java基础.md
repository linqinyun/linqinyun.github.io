# Java基础

## 基础语法

### JDK、JRE、JVM有什么区别

- JDK:Java Development Kit 针对Java程序员的产品
- JRE:Java Runtime Environment 是运行Java的环境集合
- JVM:Java虚拟机用于运行Java字节码文件（.class文件），跨平台的核心
  - 跨平台特性，JVM层面开发，又JVM转义其他平台api完成对接工作

### 常用数字类型的区别

|      名称      |                     取值范围                     | 存储空间 |
| :------------: | :----------------------------------------------: | :------: |
|   字节(Byte)   |         $-2^7$ ~ $2^7-1$ <br> -128 ~ 127         | 1个字节  |
| 短整数(short)  |      $-2^15$ ~ $2^15-1$ <br> -32768 ~ 32767      | 2个字节  |
|   整数(int)    | $-2^31$ ~ $2^31-1$ <br> -2147483648 ~ 2147483647 | 四个字节 |
|  长整数(long)  |               $-2^63$ ~  $2^63-1$                | 8个字节  |
| 单精度(float)  |               $2^-149$ ~ $2^128-1$               | 4个字节  |
| 双精度(double) |              $2^-1074$ ~ $2^1024-1$              | 8个字节  |

### Float在JVM的表达方式及使用陷阱

```java
float d1 = 423432423f;
float d2 = d1 + 1;
if(d1 == d2){
    System.out.println("d1==d2");
}else{
    System.out.println("d1!=d2");
}
```

- 打印结果为：d1 == d2
- float类型在内存中的存储形式为**科学计数法**，表达为：4.2343242E7
- d2用科学计数法表示同样为4.2343242E7，因此d1 == d2
- **注意Java类型转换**
- **大数计算使用 `BigDecimal`**

### 随机数的使用

```java
package com.lin.interview;

import java.util.Random;

/**
 * 30-100的随机整数
 */
public class RandomSample {
    //方法1
    public Integer randomInt1(){
        int min = 30;
        int max = 101;
        int result =  new Random().nextInt(max - min) + min;
        return  result;
    }
    //方法2
    public Integer randomInt2(){
        int min = 30;
        int max = 101;
        int result = (int)(Math.random()*(max-min))+min;
        return result;
    }

    public static void main(String[] args) {
        System.out.println(new RandomSample().randomInt1());
        System.out.println(new RandomSample().randomInt2());
    }
}

```

### 找出1-1000内的质数

```java
package com.lin.interview;

/**
 * 1-1000内的质数
 */
public class PrimeNumber {
    public static void main(String[] args) {
        for (int i = 2; i <= 1000; i++) {
            boolean flag = true;
            for (int j = 2; j < i; j++) {
                if (i % j == 0) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                System.out.print(i + " ");
            }
        }
    }
}

```

## 面向对象

### 面向对象三大特性是什么

- 封装
  - 封装的好处：
    1. 实现专业的分工
    2. 减少代码的耦合
    3. 可以自由修改类的内部结构
    4. 接口是封装常用使用
- 继承
  - Java中类是不支持多继承的，接口可以，一个接口可以继承多个其它接口
- 多态
  - 多态是三大特性中最重要的操作
  - 多态是同一个行为具有多个不同表现形式或形态的能力
  - 多态是同于一个接口，使用不用的实例而执行不同操作
- **补充：接口与抽象类有哪些异同**

|       相同点       |                           不同点                            |
| :----------------: | :---------------------------------------------------------: |
|   都是上层的抽象   |    抽象类可包含方法的实现 <br> 接口则只能包含方法的声明     |
|    不能被实例化    |    继承类只能继承一个抽象类 <br> 实现类可以实现多个接口     |
| 都可以包含抽象方法 |                 抽象级别 接口>抽象类>实现类                 |
|                    | 作用不同：<br> 接口用于约束程序行为 <br> 继承则用于代码复用 |

>注意：JDK8以上版本，接口可以有default方法，包含方法实现

### 静态与实例变量的区别

- 语法区别：静态变量前要加static关键字，实例则不用
- 隶属区别：实例变量属于某个对象的属性，而静态属于类
- 运行区别：静态变量在JVM加载类被创建（运行时无法被垃圾回收释放，存储区在JVM方法区，存储空间小），实例变量在实例化对象时创建（不使用时释放，存储在对象堆的内存中，存储空间多）

### 类的执行顺序

1. 静态优先
2. 父类优先
3. 非静态块优先与构造函数，**构造函数也属于非静态块**
4. 142356

### Java的异常体系

- Error -> Throwable
- RuntimeException -> Exception -> Throwable
- Exception：所有异常统称，需强制进行处理
- RuntimeException：运行时异常，编码阶段不强制处理，不显示try catch处理

#### Error和Exception的区别与联系

|         Exception          |               Error                |
| :------------------------: | :--------------------------------: |
|  可以是可被控制或不可控制  |            总是不可控制            |
| 表示一个由程序员导致的错误 | 经常用于表示系统错误或底层资源错误 |
|   应该在应用程序级被处理   |  如果可能的话，应该在系统级被捕获  |

## 字符串

### String与字符串常量

- 字符串创建后由final修饰，不可变
- 字符串默认保存在方法区域中特定开辟的区域->常量池
- 不同String对象指向相同字符串，其实同指相同的地址

```Java
String s1="abc";
String s2="def";
String s3="abc"+"def";
String s4="abcdef";
String s5=s2+"def";
String s6=new String("abc");
System.out.println(s1==s2);//true 比较地址
System.out.println(s3==s4);//true 比较地址
System.out.println(s4==s5);//false 实际使用中s2为引用类型，编译期间无法确定数值，运行时确定，s5为新的地址与s4不同
System.out.println(s4.equals(s5));//true 比较字符内容
System.out.println(s1==s6);//false new出的对象，存放地址不同
```

### String、StringBuilder与StringBuffer的区别

|          |     String     |     StringBuilder      |      StringBuffer      |
| :------: | :------------: | :--------------------: | :--------------------: |
| 执行速度 |      最差      |          其次          |          最高          |
| 线程安全 |    线程安全    |        线程安全        |       线程不安全       |
| 使用场景 | 少量字符串操作 | 多线程环境下的大量操作 | 单线程环境下的大量操作 |

## 集合

### List与Set的区别

|              |           List            |                   Set                   |
| :----------: | :-----------------------: | :-------------------------------------: |
|   允许重复   |            是             |                   否                    |
| 是否允许null |            是             |                   否                    |
|   是否有序   |            是             |                   否                    |
|    常用类    | ArrayList <br> LinkedList | HashSet <br> LinkedHashSet <br> TreeSet |

#### ArrayList与LinkedList的区别

|          |  ArrayList   |   LinkedList   |
| :------: | :----------: | :------------: |
| 存储结构 | 基于动态数组 |    基于链表    |
| 遍历方式 |   连续读取   |    基于指针    |
| 适用场景 | 大数据量读取 | 频繁新增、插入 |

#### HashSet与TreeSet的区别

|          |    HashSet     |    TreeSet     |
| :------: | :------------: | :------------: |
| 排序方式 |  不能保证顺序  | 按预置规则排序 |
| 底层存储 |  基于HashMap   |  基于TreeMap   |
| 底层实现 | 基于Hash表实现 | 基于二叉树实现 |

### List排序的编码实现

```Java
package com.lin.interview.sorter;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class ListSorter {
    public static void main(String[] args) {
        List<Employee> emps = new ArrayList<Employee>();
        emps.add(new Employee("张三",33,10009f));
        emps.add(new Employee("李四",40,7800f));
        emps.add(new Employee("王五",50,6533f));

        Collections.sort(emps, new Comparator<Employee>() { //排序
            @Override
            public int compare(Employee o1, Employee o2) { //内部匿名方法
                if((o1.getSalary()-o2.getSalary())<0 && (o1.getSalary()-o2.getSalary())>-1){
                    return -1;
                }
                if((o1.getSalary()-o2.getSalary())>0 && (o1.getSalary()-o2.getSalary())<1){
                    return 1;
                }
                // 当前为升序
                return (int)(o1.getSalary()-o2.getSalary());
                // 当前为降序
                //return (int)(o2.getSalary()-o1.getSalary());
            }
        });
        System.out.println(emps);
    }
}

```

### TreeSet排序的编码实现

```Java
public class Employee implements Comparable<Employee>{
    @Override
    public int compareTo(Employee o) {
        return this.getAge().compareTo(o.getAge()); //compareTo 1 前面大于后面 -1 后面大于前面
    }
}
public class SetSorter{
    public static void main(String[] args) {
        //直接调用
        TreeSet<Employee> emps = new TreeSet<Employee>();
    }
}

```

### hashCode与equals的联系与区别

- equals()方法用于判断两个对象是否"相同"
- hashCode()放回一个int，代表"该对象的内存地址" **不可靠**

---

1. 两个对象如果equals()成立，hashCode()一定成立吗
2. 如果equals()不成立，hashCode()可能成立
3. 如果hashCode()成立，equals()不一定成立
4. hashCode()不相等，equals()一定不成立

## 输入输出流

### Java IO中有几种类型的流

|        |                              输入流                               |                               输出流                               |
| :----: | :---------------------------------------------------------------: | :----------------------------------------------------------------: |
| 字节流 |     InputStream <br> FileInputStream <br> BufferedInputStream     |    OutputStream <br> FileOutputStream <br> BufferedOutputStream    |
| 字符流 | Reader <br> FileReader <br> InputStreamReader <br> BufferedReader | Writer <br> FileWriter <br> OutputStreamWriter <br> BufferedWriter |

### 利用IO实现文件复制

#### 举例：复制文件到指定文件夹

```Java
package com.lin.interview;

import java.io.*;

public class FileCopy {
    public static void main(String[] args) {
        //1. 利用Java IO
        File source = new File("D:\\开发资源\\spring-framework-4.2.4.RELEASE-dist.zip");
        File target = new File("D:\\开发资源\\target\\spring-framework-4.2.4.RELEASE-dist.zip");

        InputStream ipt = null;
        OutputStream opt = null;

        try {
            ipt = new FileInputStream(source);
            opt = new FileOutputStream(target);
            byte[] buf = new byte[1024];
            int byteRead;

            while ((byteRead = ipt.read(buf)) != -1) {
                opt.write(buf, 0, byteRead);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                ipt.close();
                opt.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }

        //2. FileChannel 实现文件复制
        //3. Commons IO组件实现文件复制
        //FileUtils.copyFile(Source,Target);
    }
}

```

## 多线程

### 实现多线程的三种方式

- 继承Thread父类
  
```Java
package com.lin.interview.thread;

import java.util.Random;

public class ThreadSample1 {
    public static void main(String[] args) {
        //五条线程
        Runnerl r1 = new Runnerl();
        r1.setName("张三");
        Runnerl r2 = new Runnerl();
        r2.setName("李四");
        Runnerl r3 = new Runnerl();
        r3.setName("王五");
        r1.start();
        r2.start();
        r3.start();
        //****************
    }


}
/**
 * 第一种 Thread
 */
class Runnerl extends Thread{
    @Override
    public void run(){
        Integer speed = new Random().nextInt(10)+1;
        for (int i=1;i<=1000;i++){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(this.getName()+"已前进"+(i*speed)+"米");
        }
    }
}

```

- 实现Runnable接口

```Java
package com.lin.interview.thread;

import java.util.Random;

public class ThreadSample2 {
    public static void main(String[] args) {
        Thread r1 = new Thread(new Runner2());
        r1.setName("张三");
        Thread r2 = new Thread(new Runner2());
        r2.setName("李四");
        Thread r3 = new Thread(new Runner2());
        r3.setName("王五");
        r1.start();
        r2.start();
        r3.start();
    }
}

/**
 * 第二种
 * 第二种与第一种 底层一致
 * 开发中更推荐 第二种，接口实现 ，更加友好
 */
class Runner2 implements Runnable {

    @Override
    public void run() {
        Integer speed = new Random().nextInt(10) + 1;
        for (int i = 1; i <= 100; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "已前进" + (i * speed) + "米");
        }
    }
}

```

- 实现Callable接口

```Java
package com.lin.interview.thread;

import java.util.Random;
import java.util.concurrent.*;

public class ThreadSample3 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建一个固定长度的线程池
        ExecutorService threadPool = Executors.newFixedThreadPool(3);
        Runner3 r1 = new Runner3();
        Runner3 r2 = new Runner3();
        Runner3 r3 = new Runner3();
        r1.setName("张三");
        r2.setName("李四");
        r3.setName("王五");
        Future<Long> result1 = threadPool.submit(r1);
        Future<Long> result2 = threadPool.submit(r2);
        Future<Long> result3 = threadPool.submit(r3);
        System.out.println(result1.get());
        System.out.println(result2.get());
        System.out.println(result3.get());
    }
}
/**
 * 第三种
 * JDK 1.5以上
 * 高级做法
 */
class Runner3 implements Callable<Long>{
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public Long call() throws Exception {
        Integer speed = new Random().nextInt(10) + 1;
        Long distince = 0l;
        for (int i = 1; i <= 100; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            distince = (long)(i*speed);
            System.out.println(this.getName() + "已前进" + (i * speed) + "米");
        }
        return distince;
    }
}

```

>主线程（守护线程）、回收线程（JVM回收资源）

### 请阐述线程的状态及触发机制

#### 线程有哪些状态

1. 新建(new)
2. 就绪(ready)
3. 运行(running)
4. 阻塞(blocked)
5. 死亡(dead)

![生命周期](./resource/j1.bmp)

### 实现线程同步的三种方式

- synchronized关键字（只能有一个线程进行访问）
  - 方法前增加synchronized关键字
  - synchronized块，进行包裹
- wait()[进入等待状态]与notify()[进入激活状态]
  - 手动进行
  - notify，仅能唤醒一个线程即默认等待队列第一个线程
  - notifyAll，不是唤醒所有，而是根据某种策略线程去竞争
- Lock JDK5以上并发支持 JUC
  - 重入锁来进行同步

#### 补充

1. ①初始(NEW)：新创建了一个线程对象，但还没有调用start()方法。
②运行(RUNNABLE)：Java线程中将就绪（ready）和运行中（running）两种状态笼统的成为“运行”。
线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取cpu 的使用权，此时处于就绪状态（ready）。就绪状态的线程在获得cpu 时间片后变为运行中状态（running）。
③阻塞(BLOCKED)：表线程阻塞于锁。
④等待(WAITING)：进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断）。
⑤超时等待(TIME_WAITING)：该状态不同于WAITING，它可以在指定的时间内自行返回。
⑥终止(TERMINATED)：表示该线程已经执行完毕。
2. 重量级锁指的就是一般意义上synchronized的同步方式，通过对象内部的监视器（monitor）实现，其中monitor的本质是依赖于底层操作系统的Mutex Lock实现，操作系统实现线程之间的切换需要从用户态到内核态的切换， 切换成本非常高。
3. 线程的切换是进程切换的基础。
每一个进程都包含一个映射表，如果进程切换了，那么程序选择的映射表肯定也不一样；进程的切换其实是包含两个部分的，第一个指令的切换，第二个映射表的切换。指令的切换就是从这段程序跳到另外一段程序执行，映射表切换就是执行不同的进程，所选择的映射表不一样。线程的切换只有指令的切换，同处于一个进程里面，不存在映射表的切换。进程的切换就是在线程切换的基础上加上映射表的切换。

### 死锁的产生

> 多线程编程中，多线程访问资源，资源没有及时释放，资源交叉引用，导致程序假死。

```Java
//线程死锁例子
package com.lin.interview;

public class DeadLock {
    private static String fileA = "A文件";
    private static String fileB = "B文件";

    public static void main(String[] args) {

        new Thread(){ //线程1
            public void run(){
                while(true) {
                    synchronized (fileA) {//打开文件A，线程独占
                        System.out.println(this.getName() + ":文件A写入");
                        synchronized (fileB) {
                            System.out.println(this.getName() + ":文件B写入");
                        }
                        System.out.println(this.getName() + ":所有文件保存");
                    }
                }
            }
        }.start();

        new Thread(){ //线程2
            public void run(){
                while(true) {
                    synchronized (fileB) {//打开文件B，线程独占
                        System.out.println(this.getName() + ":文件B写入");
                        synchronized (fileA) {
                            System.out.println(this.getName() + ":文件A写入");
                        }
                        System.out.println(this.getName() + ":所有文件保存");
                    }
                }
            }
        }.start();
    }
}

```

## 垃圾回收与JVM内存

### JVM的内存组成

![JVM内存组成](./resource/j2.bmp)

>面试常考，JVM内存中通用回答，五大区域

- 数据共享区-所有线程共享
- 方法区：虚拟机加载的**类、常量、静态变量、方法声明**等数据。
- 堆：**Java变量**，存放程序运行时的**对象实例**，是垃圾回收**主要区域，为最大区域**。

---

- 数据私有区-线程独享
- 程序计数器PC：**行号计数器**，程序涉及 分支、跳转、循环、异常处理、线程恢复。
- 虚拟机栈：**为Java方法服务**，为每一个方法创建一个栈帧 **[方法执行的状态]**，（栈帧）可以看出对方法的引用，每个栈帧在内存中的实例。
- 本地方法栈：**为执行本地方法服务**，为操作系统级别的底层方法。

### GC垃圾回收及算法介绍

- GC(Garbage Collection)用于回收不再使用的内存
- GC负责3项任务：分配内存、确保引用、回收内存
- GC回收的依据，某个对象没有任何引用，则可以被回收

>GC是一把双刃剑，一方面程序开发提供便捷，另一方面不断地对内存进行监听等，增加JVM负担，降低程序执行效率。

#### GC回收算法

1. 引用计数算法
   1. 对象计数，无计数则回收
   2. 两个对象相互引用，出现循环引用，此算法无法解决
   3. 最简单，效率最低
2. 跟踪回收算法
   1. JVM维护的对象引用图
   2. 遍历结束后，没有被遍历的对象则回收
3. 压缩回收算法
   1. 将JVM活动的对象放置集中区域中
   2. 性能损失大
4. 复制回收算法
   1. 将堆分成两个相同大小的区域
   2. 任何时刻，只有一个区域使用，直至使用完
   3. 中断程序运行，通过遍历
   4. 活动对象紧密在一起复制到另一个区域中
   5. 清理碎片
   6. 既清理又整理，访问、寻址效率高
   7. 需要空间大，需要中断程序，降低程序效率
5. **按代回收算法**
   1. 主流算法
   2. 解决复制回收算法问题
   3. 将堆分成多个子堆，每个子堆为一代。
   4. 优先收集年轻对象，多次使用对象，将对象移入高一级堆中
   5. 低代（扫描频率高）->高代（扫描频率低）

### Java的内存泄露场景

>Java存在内存泄漏
---
>内存泄漏:一个不再程序被使用的对象、变量还在内存占用空间
---

1. 静态集合类
   1. 存放数据量比较大
   2. 没有回收导致泄漏
2. 各种连接
   1. 数据库连接、网络连接、IO连接等
   2. 打开未被关闭导致不会被回收
3. 监听器
   1. 监听器全局存在
   2. 监听对象没有有效控制
4. 不合理作用域
   1. **作用域最小化原则**为软件开发原则
   2. 变量定义范围大于使用范围
   3. 没有把引用对象设置null，导致内存泄漏，情况极少发生。

### 请实现对象浅复制与深复制

>Object中存在一个对象复制，这里的复制指浅复制

- 浅复制：只对对象及变量值进行复制，引用对象地址不变
- 深复制：不仅对象及变量进行复制，引用对象也进行复制

```Java
package com.lin.interview.clone;

import java.io.*;

public class Dancer implements Cloneable,Serializable{
    private String name;
    private Dancer partner;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Dancer getPartner() {
        return partner;
    }

    public void setPartner(Dancer partner) {
        this.partner = partner;
    }

    //深复制 采用序列化方式
    public Dancer deepClone() throws IOException, ClassNotFoundException {
        //序列化,将内存中的对象序列化为字节数组
        ByteArrayOutputStream bos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bos);
        oos.writeObject(this);

        //反序列化，将字节数组转回为对象，同时完成深复制的任务
        ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
        ObjectInputStream ois = new ObjectInputStream(bis);
        return (Dancer)ois.readObject();
    }

    public static void main(String[] args) throws CloneNotSupportedException, IOException, ClassNotFoundException {
        Dancer d1 = new Dancer();
        d1.setName("刘明");
        Dancer d2 = new Dancer();
        d2.setName("吴明霞");
        d1.setPartner(d2);
        System.out.println("Partner:" + d2.hashCode());
        //浅复制
        Dancer shallow = (Dancer) d1.clone();
        System.out.println("浅复制："+shallow.getPartner().hashCode());
        //深复制
        Dancer deep = (Dancer)d1.deepClone();
        System.out.println("深复制："+deep.getPartner().hashCode());
    }
}

```
