幂等

- 请求幂等
  - 转账
- 业务幂等
  - 重复下单，同一时间段内（）



请求幂等深度分析（请求类型层面CRUD）

- Create/Insert
  - 自增id
  - `uid` 业务id + unique 主键
- Read/Select
  - 天然幂等
- Update（多线程场景下）
  - `aba`问题
    - 基于乐观锁（版本号），数据更新成功，影响行数为0
    - 线程 A update -> 线程 B update -> 线程A请求响应失败重试，数据状态不一致
- Delete（多线程场景下）
  - 绝对值删除，存在aba问题
    - A -> 插入
    - B -> 删除
  - 同一条情况下，幂等
  - 相对值删除，修改
    - 想办法，将其改为绝对值的删除和修改，再处理aba问题



电商下订单企业级案例剖析

- Insert 请求 -> 可用 业务主键生成
- `OrderID` 如何产生

订单生成层推导

- `app` -> db
  - 只能在 业务逻辑层生成
- 使用`redis`
  - `uuid` 标识请求（`app`端生成）

![idempotent](https://raw.githubusercontent.com/jinminer/docs/master/on-the-way/idempotent/1.0-idempotent.png)