- `CAS`

  什么是`CAS`？怎么解决`ABA`问题？

  `lock cmpxchg` 指令

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
  
  新建一个Object对象占用多少个字节？

