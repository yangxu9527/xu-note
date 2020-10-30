# xu-note
- 计算机基础
- JAVA基础
- 数据结构和算法
- 设计模式
- JVM
  - 内存结构
    - 程序计数器
    - 虚拟机栈
    - 本地方法栈
    - 堆
    - 方法区
    - 直接内存
- JAVA多线程高并发

  - 进程与线程
    - 进程和线程对比
    - 并行与并发
  - JAVA线程
    - 创建和运行线程（三种方法）
    - 查看进程和线程的命令
    - 栈和栈帧
    - 常见方法
    - 守护线程
    - 线程的五种状态（操作系统）
    - 线程的六种状态以及转换（JAVA）
  - 共享模型之管程
    - 临界区和竞态条件
    - synchronized和线程八锁
    - 线程安全分析
    - Monitor
    - wait & notify
    - park & unpark
    - 多把锁
    - 死锁和活锁
  - 共享模型之内存
  - 共享模型之无锁
  - 共享模型之不可变
  - 共享模型之工具
- `Spring`
- `Spring Cloud`
- `Mybatis`
- `Mysql`
- `Redis`
- 消息队列
- 经典面试题

------

#### 常用命令

- 保存堆内存快照文件：`jmap -dump:live,format=b,file=<filename> <PID>`
- 查看进程：`top`

- 查看线程占用资源情况：`ps H -eo pid,tid,%cpu,%mem |grep <PID>`
- 查看栈日志：`jstack <PID>`



------

课程：

（1）并发基础：https://www.bilibili.com/video/BV1V4411p7EF?from=search&seid=3162053111718328162

（2）并发高级：https://www.bilibili.com/video/BV1xK4y1C7aT?p=2