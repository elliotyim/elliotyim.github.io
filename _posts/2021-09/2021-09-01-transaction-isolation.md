---
layout: post
title: Transaction Isolation
description: DB Transaction Isolation
date: 2021-09-01 09:20
---

# Transaction Isolation

![TransactionIsolation](/assets/images/transaction-isolation/isolation-level.png)

- 격리 수준(isolation level)은 동시에 여러 트랜잭션이 처리될 때 트랜잭션끼리 얼마나 서로 고립되어 있는지를 나타내는 것
- ANSI/ISO SQL Standard에서 정의한 격리수준은 4가지로 나뉘며, 아래로 내려갈 수록 고립 정도가 높아지며 성능이 떨어진다.
  - Level 0: READ UNCOMMITTED
  - Level 1: READ COMMITTED
  - Level 2: REPEATABLE READ
  - Level 3: SERIALIZABLE READ
- 일반적인 온라인 서비스에서는 READ COMMITTED나 REPEATABLE READ 중 하나를 사용
- 격리수준은 DB의 ACID 특성을 보장하기 위해서 사용한다
- 무조건적인 Locking은 성능저하를 가져오고, 느슨한 Locking은 데이터 무결성에 큰 문제를 가져오기 때문에 효율적인 Locking을 사용하기 위해 적절한 격리수준은 반드시 필요하다

## READ UNCOMMITTED

---

![read-uncommitted](/assets/images/transaction-isolation/read-uncommitted.png)

- 각 트랜잭션에서의 변경 내용이 COMMIT이나 ROLLBACK 여부에 상관 없이 다른 트랜잭션에서 값을 읽을 수 있다
- 정합성에 문제가 많은 격리 수준이기 때문에 잘 사용되지 않는다
- 위의 그림 처럼 COMMIT 되지 않은 상태임에도 UPDATE된 값을 다른 트랜잭션에서 읽을 수 있다
- Dirty Read 현상이 발생한다
  - 작업이 커밋되지 않은 트랜잭션의 데이터를 다른 트랜잭션에서 읽을 수 있는 현상
- ORACLE은 이 레벨을 지원하지 않는다

## READ COMMITTED

---

![read-committed](/assets/images/transaction-isolation/read-committed.png)

- RDB에서 기본적으로 사용되고 있는 격리수준으로 Dirty Read 현상이 발생하지 않는다
- Select문이 실행되는 동안 Shared Lock이 걸리고 조회 시에는 실제 테이블의 값을 가져오는 것이 아닌 Undo 영역에 백업된 레코드에서 값을 가져온다
- 하나의 트랜잭션에서 똑같은 Select 쿼리를 실행했을 때는 항상 같은 결과를 가져와야 하는 REPEATABLE READ의 정합성에 어긋난다
  - Non-repeatable Read
    - 다른 트랜잭션에서 같은 쿼리로 2번 이상 조회했을 때 그 결과가 상이한 상황을 말한다
    - 보통 데이터의 수정 및 삭제가 발생했을 경우에 일어난다

## REPEATABLE READ

---

![RepeatableRead](/assets/images/transaction-isolation/repeatable-read.png)

- MySQL에서 기본으로 제공하는 격리수준으로 트랜잭션마다 트랜잭션 ID를 부여하여 해당 트랜잭션 ID보다 낮은 트랜잭션 번호에서 변경한 것만 읽는 격리수준이다
  - 트랜잭션이 시작되기 전에 커밋된 내용만 조회할 수 있다
- Undo 공간에 백업해두고 실제 레코드 값을 변경한다
  - 백업된 데이터는 불필요하다고 판단하는 시점에 주기적으로 삭제한다
  - Undo에 백업된 레코드가 많아지면 MySQL 서버의 처리 성능이 떨어질 수 있다
- 이러한 변경방식을 MVCC(Multi Version Concurrency Control)라고 부른다
- 트랜잭션이 시작 시점 데이터의 일관성을 보장해야 하기 때문에 트랜잭션의 실행시간이 길어질수록 계속 멀티 버전을 관리해야 하는 단점이 있다
- Phantom Read가 발생할 수 있다.

### Phantom Read

---

![PhantomRead](/assets/images/transaction-isolation/phantom-read.png)

- 한 트랜잭션 내에서 같은 쿼리를 두 번 실행했는데 첫 번째 쿼리에서 없던 유령(Phantom) 레코드가 두 번째 쿼리에서 나오는 현상을 말한다
- REPEATABLE READ 이하에서만 발생하고 (SERIALIZABLE은 발생하지 않는다), INSERT에 대해서만 발생한다
- 이를 방지하기 위해서는 쓰기 잠금을 걸어야 한다

## SERIALIZABLE

---

- 가장 단순한 격리 수준이지만 가장 엄격한 격리 수준
- 읽기 작업에도 공유 잠금을 설정하게 되고, 동시에 다른 트랜잭션에서 한 레코드를 변경하지 못하게 되기 때문에 성능 측면에서는 동시처리성능이 가장 낮다
- SERIALIZABLE에서는 PHANTOM READ가 발생하지 않지만 일반적으로 데이터베이스에서는 거의 사용되지 않는다

# References

- https://joont92.github.io/db/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80-isolation-level/
- https://akasai.space/what-is-isolation-level/
- https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation
- http://wiki.gurubee.net/pages/viewpage.action?pageId=21200923
