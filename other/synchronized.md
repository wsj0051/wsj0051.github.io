# synchronized
对于锁的使用，能够让并发变成序列化
critical section 临界区：没必要放进临界区的代码，一定不要放
锁代码：持有锁的时候才能执行这段代码
原子性：上锁之后，代码不可被打断（持有同样锁的其他代码，打断指的是同时运行）
## 锁在代码中如何实现

保持某一段代码的原子性

## synchronized代码
```java
Object o = new Object();
ClassLayout.parseInstance(o).toPrintable();
```
```xml
<dependency>
    <groupId>org.openjdk.jol</groupId>
    <artifactId>jol-core</artifactId>
    <version>0.9</version>
</dependency>
```