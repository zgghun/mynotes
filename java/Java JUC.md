# java.util.concurrent

## 1. volatile关键字

> 当多个线程操作同一共享数据时，彼此是不可见的，而volatile可实现内存可见性  

volatile与synchronized相比

1. 相比于synchronized，volatile是一种较为轻量级的同步策略
2. volatile不具备“互斥性”
3. volatile不能保证变量的“原子性”

### 2. Atomic与CAS算法

Atomic-原子变量：jdk1.5之后，java.util.concurrent.atomic包下提供了常用的原子变量  

