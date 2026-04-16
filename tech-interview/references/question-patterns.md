# 技术面试题库

> 来源：CyC2018/CS-Notes、Snailclimb/JavaGuide、haizlin/fe-interview、xiaolincoding（小林coding）、doocs/advanced-java 等开源面试资料整理

## 一、前端

### 1. 说一下 JavaScript 中的事件循环（Event Loop）机制
- **难度**: 中级
- **考察要点**:
  - 区分宏任务（setTimeout、setInterval、I/O）与微任务（Promise.then、MutationObserver、queueMicrotask）
  - 执行顺序：同步代码 → 微任务队列清空 → 宏任务取一个 → 微任务队列清空 → 循环
  - 能否结合 Node.js 的 Event Loop 阶段（timers、poll、check）做对比
  - 能否给出经典代码题的正确输出顺序

### 2. Vue 的响应式原理是什么？Vue 2 和 Vue 3 有什么区别？
- **难度**: 中级
- **考察要点**:
  - Vue 2：Object.defineProperty 做 getter/setter 劫持，Dep + Watcher 依赖收集
  - Vue 3：Proxy 实现，解决无法检测属性新增/删除、数组索引修改的问题
  - effect、track、trigger 等核心函数
  - 懒执行、调度器（scheduler）的作用

### 3. React 中 setState 是同步还是异步的？React 18 有什么变化？
- **难度**: 中级
- **考察要点**:
  - React 17 及之前：合成事件和生命周期中「批量更新」（表现为异步），setTimeout/原生事件中同步
  - React 18 Automatic Batching：所有场景默认批量更新
  - flushSync 强制同步刷新
  - 底层：更新队列、Lane 模型优先级调度

### 4. 浏览器从输入 URL 到页面渲染完成经历了哪些步骤？
- **难度**: 中级
- **考察要点**:
  - DNS 解析 → TCP 三次握手 → TLS 握手 → HTTP 请求/响应
  - HTML 解析 DOM 树、CSS 解析 CSSOM 树
  - Render Tree → Layout → Paint → Composite
  - 关键渲染路径优化、defer/async 差异、preload/prefetch/preconnect

### 5. 什么是闭包？应用场景和可能的内存问题？
- **难度**: 初级
- **考察要点**:
  - 定义：函数能记住并访问其词法作用域，即使在作用域之外执行
  - 场景：模块模式、柯里化、防抖节流、事件回调保存状态
  - 内存泄漏：闭包持有外部变量引用导致 GC 无法回收
  - 经典题：for 循环中 var 与 let 的区别

### 6. CSS 中 BFC 是什么？如何触发？解决什么问题？
- **难度**: 初级
- **考察要点**:
  - BFC：独立渲染区域，内部布局不影响外部
  - 触发：float 不为 none、overflow 不为 visible、display flex/inline-block、position absolute/fixed
  - 解决：清除浮动、防止 margin 重叠、阻止浮动覆盖

### 7. React Fiber 架构解决了什么问题？核心原理是什么？
- **难度**: 高级
- **考察要点**:
  - 解决 Stack Reconciler 同步递归渲染阻塞主线程
  - Fiber 可中断工作单元，链表结构（child/sibling/return）
  - 双缓冲：current tree 与 workInProgress tree
  - 时间切片 + Lane 模型优先级调度

### 8. 前端性能优化：从网络层、渲染层、代码层分别说明
- **难度**: 中级
- **考察要点**:
  - 网络层：CDN、HTTP/2、Gzip/Brotli、缓存策略
  - 渲染层：减少回流重绘、合成层优化、虚拟列表
  - 代码层：Tree Shaking、Code Splitting、懒加载、Web Worker
  - 结合 Lighthouse 指标（FCP、LCP、CLS、TTI）

### 9. Promise 实现原理，以及 all/race/allSettled 区别
- **难度**: 中级
- **考察要点**:
  - 三种状态不可逆，then 返回新 Promise 实现链式调用
  - all：全 fulfilled 才 resolve，任一 rejected 即 reject
  - race：第一个 settled 的结果
  - allSettled：等待所有 settled，返回每个状态和值
  - 加分：能手写简易版

### 10. 跨域问题怎么产生？有哪些解决方案？
- **难度**: 初级
- **考察要点**:
  - 同源策略：协议、域名、端口三者一致
  - CORS：简单请求 vs 预检请求（OPTIONS）
  - JSONP：script 标签不受同源限制，只支持 GET
  - 代理：Nginx 反向代理、Node.js 中间层、webpack devServer proxy

---

## 二、后端

### 1. Java 中 HashMap 的底层实现原理？JDK 1.8 做了哪些优化？
- **难度**: 中级
- **考察要点**:
  - 数组 + 链表 + 红黑树（JDK 1.8）
  - hash 计算：高 16 位异或低 16 位
  - 链表长度 > 8 且数组 ≥ 64 转红黑树；≤ 6 退化链表
  - 扩容：负载因子 0.75、容量翻倍、高位判断新位置
  - 线程不安全，应使用 ConcurrentHashMap

### 2. Go 语言的 GMP 调度模型是什么？
- **难度**: 高级
- **考察要点**:
  - G（Goroutine）、M（OS Thread）、P（逻辑处理器）
  - P 的本地队列 + 全局队列 + work stealing
  - Go 1.14 基于信号的异步抢占
  - sysmon 监控线程

### 3. Spring Boot 的自动配置原理是什么？
- **难度**: 中级
- **考察要点**:
  - @EnableAutoConfiguration → SpringFactoriesLoader 加载 spring.factories
  - @Conditional 系列注解决定配置是否生效
  - 自定义 starter 实现步骤

### 4. 什么是 RESTful API？如何设计好的 RESTful API？
- **难度**: 初级
- **考察要点**:
  - 无状态、统一接口、资源导向
  - URL 名词复数、HTTP 方法语义、状态码规范
  - 版本控制、分页、过滤
  - 对比 GraphQL、gRPC 适用场景

### 5. 微服务中服务通信和分布式事务如何处理？
- **难度**: 高级
- **考察要点**:
  - 同步：HTTP/REST、gRPC；异步：消息队列
  - 分布式事务：2PC/3PC、TCC、Saga、本地消息表
  - CAP 定理与 BASE 理论权衡

### 6. JVM 垃圾回收机制？G1 和 ZGC 的区别？
- **难度**: 高级
- **考察要点**:
  - 分代模型、标记-清除/复制/整理
  - G1：Region 化、Mixed GC、可预测停顿
  - ZGC：着色指针、读屏障、亚毫秒级停顿

### 7. Python 的 GIL 是什么？如何实现真正的并行？
- **难度**: 中级
- **考察要点**:
  - GIL：同一时刻只有一个线程执行字节码
  - 绕过：multiprocessing、C 扩展释放 GIL、concurrent.futures
  - Python 3.13 free-threaded mode

### 8. 常见的限流算法有哪些？
- **难度**: 中级
- **考察要点**:
  - 固定窗口、滑动窗口、漏桶、令牌桶
  - 分布式限流：Redis + Lua
  - 多维度限流策略

### 9. Go 中 channel 和 mutex 分别适用什么场景？
- **难度**: 中级
- **考察要点**:
  - Channel：CSP 模型，goroutine 通信、流水线
  - Mutex：保护共享资源读写
  - 常见陷阱：channel 泄漏、死锁、nil channel

### 10. 如何设计幂等接口？
- **难度**: 中级
- **考察要点**:
  - 天然幂等：GET、PUT、DELETE
  - POST 幂等方案：唯一请求 ID + 去重、数据库唯一约束、状态机、Token 机制

---

## 三、数据库

### 1. MySQL 索引为什么用 B+ 树而不用 B 树或哈希表？
- **难度**: 中级
- **考察要点**:
  - B+ 树叶子节点链表连接，范围查询高效；非叶子只存 key，树更矮
  - 哈希不支持范围查询；B 树数据分散在所有节点
  - 聚簇索引 vs 二级索引回表

### 2. MySQL 事务隔离级别和 MVCC 实现原理？
- **难度**: 高级
- **考察要点**:
  - 四种隔离级别及解决的问题
  - MVCC：trx_id、roll_pointer、Undo Log 版本链、ReadView
  - RC 每次 SELECT 新建 ReadView，RR 事务首次 SELECT 生成
  - RR 级别 MVCC + Next-Key Lock 解决幻读

### 3. Redis 为什么快？单线程模型优缺点？
- **难度**: 中级
- **考察要点**:
  - 内存操作、高效数据结构、IO 多路复用
  - 6.0 多线程 IO（命令执行仍单线程）
  - 缺点：单个耗时命令阻塞所有请求

### 4. MySQL 慢查询如何排查和优化？
- **难度**: 中级
- **考察要点**:
  - 慢查询日志、EXPLAIN 分析
  - 索引优化：覆盖索引、最左前缀、避免索引失效
  - 深分页优化：延迟关联/游标分页

### 5. Redis RDB 和 AOF 如何选择？
- **难度**: 中级
- **考察要点**:
  - RDB：快照 + fork COW，恢复快但可能丢数据
  - AOF：追加日志，三种刷盘策略
  - 4.0 混合持久化

### 6. Redis 缓存穿透、击穿、雪崩的解决方案？
- **难度**: 中级
- **考察要点**:
  - 穿透：布隆过滤器、缓存空值
  - 击穿：互斥锁、逻辑过期
  - 雪崩：过期加随机值、多级缓存、熔断降级

### 7. MySQL 锁机制和死锁排查？
- **难度**: 高级
- **考察要点**:
  - 表锁、行锁（Record/Gap/Next-Key Lock）、意向锁
  - 死锁四条件和排查方法

### 8. MongoDB 适合什么场景？与 MySQL 对比？
- **难度**: 中级
- **考察要点**:
  - 文档型、Schema 灵活、横向扩展
  - 适合日志、内容管理、IoT、快速迭代业务

### 9. 分库分表方案和中间件？
- **难度**: 高级
- **考察要点**:
  - 垂直拆分、水平拆分、分片键选择
  - 分布式 ID（Snowflake）、跨片查询
  - ShardingSphere、Vitess

### 10. Redis 与 MySQL 数据一致性如何保证？
- **难度**: 高级
- **考察要点**:
  - Cache Aside：先更新 DB 再删缓存
  - 延迟双删、Canal 订阅 binlog
  - 强一致性：分布式锁

---

## 四、系统设计

### 1. 设计短链接系统
- **难度**: 中级
- **考察要点**: Base62 编码、301 vs 302、Redis 缓存、发号器集群

### 2. 设计分布式限流系统
- **难度**: 高级
- **考察要点**: Redis 滑动窗口/令牌桶 Lua、多维度限流、网关层 vs 应用层

### 3. 设计消息队列系统
- **难度**: 高级
- **考察要点**: 消息可靠性/顺序性/幂等消费、堆积处理、Kafka vs RocketMQ vs RabbitMQ

### 4. 设计秒杀系统
- **难度**: 高级
- **考察要点**: 前端静态化+CDN、Redis Lua 预减库存、MQ 异步下单、熔断降级

### 5. 设计分布式 ID 生成器
- **难度**: 中级
- **考察要点**: UUID/自增/Redis INCR/Snowflake 各自优劣、时钟回拨

### 6. 如何保证系统高可用？
- **难度**: 高级
- **考察要点**: 冗余、负载均衡、熔断降级、监控告警、SLA 指标

### 7. 设计分布式缓存系统
- **难度**: 高级
- **考察要点**: 本地缓存+Redis Cluster、一致性哈希/虚拟节点、热点 key/大 key

### 8. 设计实时通知系统
- **难度**: 中级
- **考察要点**: 长轮询/SSE/WebSocket、连接管理、离线消息

### 9. CAP 定理和 BASE 理论？实际如何取舍？
- **难度**: 中级
- **考察要点**: CP vs AP 举例、核心交易强一致性、非核心最终一致性

### 10. 设计 Feed 流系统
- **难度**: 高级
- **考察要点**: 推模式/拉模式/推拉结合、Redis SortedSet、游标分页

---

## 五、计算机基础

### 1. TCP 三次握手和四次挥手？为什么握手三次挥手四次？
- **难度**: 中级
- **考察要点**: SYN/ACK 流程、TIME_WAIT 2MSL、大量 TIME_WAIT/CLOSE_WAIT 排查

### 2. 进程、线程、协程的区别？
- **难度**: 初级
- **考察要点**: 资源分配 vs CPU 调度 vs 用户态调度、各语言协程实现

### 3. HTTP 1.0/1.1/2.0/3.0 区别？
- **难度**: 中级
- **考察要点**: 持久连接、多路复用、HPACK、QUIC/UDP、TLS 握手

### 4. 常见排序算法复杂度和稳定性？快排原理？
- **难度**: 初级
- **考察要点**: 各排序复杂度对比、快排 partition、优化技巧

### 5. 什么是死锁？如何避免？
- **难度**: 中级
- **考察要点**: 四必要条件、银行家算法、固定加锁顺序、排查工具

### 6. TCP 和 UDP 区别？各适用什么场景？
- **难度**: 初级
- **考察要点**: 可靠/不可靠、拥塞控制、QUIC/KCP

### 7. 什么是一致性哈希？解决什么问题？
- **难度**: 中级
- **考察要点**: 哈希环、虚拟节点、对比哈希槽方案

### 8. 虚拟内存如何工作？
- **难度**: 高级
- **考察要点**: 页表映射、缺页中断、页面置换算法、TLB、多级页表

### 9. LRU 缓存实现原理？
- **难度**: 中级
- **考察要点**: 哈希表+双向链表 O(1)、手写实现、Redis 近似 LRU

### 10. HTTPS 如何保证安全？TLS 握手过程？
- **难度**: 中级
- **考察要点**: 对称+非对称加密、TLS 1.2/1.3 握手、CA 证书链、前向安全性

---

## 难度校准参考

| 资历 | 基础题难度 | 项目追问深度 | 设计题复杂度 |
|------|-----------|-------------|-------------|
| 1-2 年 | 初级题为主 | 关注实现细节 | 简单编码题 |
| 3-5 年 | 中级题 + 原理对比 | 架构决策 + 权衡 | 单模块系统设计 |
| 5-8 年 | 高级题 + 边界场景 | 全局视角 + 推动力 | 完整系统设计 |
| 8+ 年 | 架构哲学 | 技术战略 + 团队 | 复杂分布式系统 |
