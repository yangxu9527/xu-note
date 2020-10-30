#### 常用工具

- `	JUC`并发压力测试工具：https://wiki.openjdk.java.net/display/CodeTools/jcstress

- `JUC`并发性能测试工具：http://openjdk.java.net/projects/code-tools/jmh/

- 对象的内存布局

  工具：`JOL`

  ```xml
  <dependency>
      <groupId>org.openjdk.jol</groupId>
      <artifactId>jol-core</artifactId>
      <version>put-the-version-here</version>
  </dependency>
  ```
  
  ```java
  ClassLayout.parseInstance(obj).toPrintable()
  ```
  
- `idea`查看`java`字节码插件：`jclass`

- `idea`查看`java`字节码插件：`asm bycode outline`

------





