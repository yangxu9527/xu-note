- **<font color=green>事务的隔离级别</font>**

  | 事务级别           | 名称     | 存在的问题         | 备注          |
  | ------------------ | -------- | ------------------ | ------------- |
  | `read-uncommitted` | 未提交读 | 脏读、重复读、幻读 | A事务可以读取 |
  | `read-committed`   | 已提交读 | 重复读、幻读       |               |
  | `repeatable-read`  | 可重复读 | 幻读               |               |
  | `serializable`     | 串行化   |                    |               |

  **脏读：**A事务可以读取到其他事务未提交的数据。

  **重复读：**A在事务期间可以读取到其他事务已提交的数据。

  **幻读：**A事务在事务期间更新所有数据，此时其他事务插入数据，导致A事务提交后发现还有未更新到的数据。（串行化可解决，在A事务对表的数据操作时，其他事务必须等到A事务提交后才能执行）。

- **<font color=green>事务相关命令</font>**

  ```mysql
  # 查看事务的隔离级别
  select @@tx_isolation;
  # MySQL默认操作模式就是autocommit自动提交模式。这就表示除非显式地开始一个事务，否则每个查询都被当做一个单独的事务自动执行。我们可以通过设置autocommit的值改变是否是自动提交autocommit模式。
  # 查看autocommit模式
  show variables like 'autocommit';
  # 关闭自动提交,每次sql必须通过commit命令提交
  set autocommit = 0;
  # 设置事务的隔离级别
  set session tx_isolation='read-uncommitted';
  # 开启一个一个事务
  start transaction;
  # 提交事务
  commit;
  # 回滚
  rollback;
  ```

- **<font color=green>Spring事务的传播行为</font>**

  | 行为类型          | 说明                                                         |
  | ----------------- | ------------------------------------------------------------ |
  | REQUIRED          | 支持当前事务，如果当前有事务， 那么加入事务， 如果当前没有事务则新建一个(默认情况)。 |
  | REQUIRES_NEW      | 支持当前事务，如果当前有事务，则挂起当前事务，然后新创建一个事务，如果当前没有事务，则自己创建一个事务。 |
  | SUPPORTS          | 如果当前有事务则加入，如果没有则不用事务。                   |
  | NOT_SUPPORTED     | 以非事务方式执行操作，如果当前存在事务就把当前事务挂起，执行完后恢复事务（忽略当前事务）。 |
  | MANDATORY         | 支持当前事务，如果当前没有事务，则抛出异常。（当前必须有事务） |
  | NESTED            | 如果当前存在事务，则嵌套在当前事务中。如果当前没有事务，则新建一个事务自己执行（和required一样）。嵌套的事务使用保存点作为回滚点，当内部事务回滚时不会影响外部事物的提交；但是外部回滚会把内部事务一起回滚回去。（这个和新建一个事务的区别） |
  | PROPAGATION_NEVER | 以非事务方式执行，如果当前存在事务，则抛出异常。（当前必须不能有事务） |

  示例：假如在方法A中调用B和C方法，以下为不同行为类型的表现。具体示例代码参见：https://github.com/yangxu9527/spring-boot-demo 中的`tx`。

  | 方法A                                   | 方法B                | 方法C                | 表现                                                         |
  | --------------------------------------- | -------------------- | -------------------- | ------------------------------------------------------------ |
  | 无事务，异常                            | REQUIRED，无异常     | REQUIRED，无异常     | 不会回滚。A方法的异常不影响B、C方法的独立事务。              |
  | 无事务，无异常                          | REQUIRED，无异常     | REQUIRED，异常       | 方法C会回滚。C的异常不影响B的正常运行。                      |
  | REQUIRED，异常                          | REQUIRED，无异常     | REQUIRED，无异常     | 整体回滚。                                                   |
  | REQUIRED，无异常                        | REQUIRED，无异常     | REQUIRED，异常       | 整体回滚。                                                   |
  | REQUIRED，无异常，捕获C的异常但不做处理 | REQUIRED，无异常     | REQUIRED，异常       | 会发生异常：`UnexpectedRollbackException: Transaction silently rolled back because it has been marked as rollback-only` |
  | 无事务，异常                            | REQUIRES_NEW，无异常 | REQUIRES_NEW，无异常 | 不会回滚。A方法的异常不影响B、C方法的独立事务。              |
  | 无事务，无异常                          | REQUIRES_NEW，无异常 | REQUIRED_NEW，异常   | 方法C会回滚。C的异常不影响B的正常运行。                      |
  | REQUIRED，异常                          | REQUIRES_NEW，无异常 | REQUIRES_NEW，无异常 | B、C的不会回滚，A的异常不影响B、C。                          |
  | REQUIRED，无异常                        | REQUIRES_NEW，无异常 | REQUIRES_NEW，异常   | C的异常不会影响到B。                                         |
  | REQUIRED，无异常，捕获C的异常但不做处理 | REQUIRES_NEW，无异常 | REQUIRES_NEW，异常   | C的异常不会影响到B。                                         |

  

