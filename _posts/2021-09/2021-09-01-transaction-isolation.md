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

![read-uncommitted](/assets/images/transaction-isolation/read-uncommitted.png)

- 각 트랜잭션에서의 변경 내용이 COMMIT이나 ROLLBACK 여부에 상관 없이 다른 트랜잭션에서 값을 읽을 수 있다
- 정합성에 문제가 많은 격리 수준이기 때문에 잘 사용되지 않는다
- 위의 그림 처럼 COMMIT 되지 않은 상태임에도 UPDATE된 값을 다른 트랜잭션에서 읽을 수 있다
- DIRTY READ 현상이 발생한다
  - 작업이 커밋되지 않은 트랜잭션의 데이터를 다른 트랜잭션에서 읽을 수 있는 현상
- ORACLE은 이 레벨을 지원하지 않는다

# References

- https://joont92.github.io/db/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EA%B2%A9%EB%A6%AC-%EC%88%98%EC%A4%80-isolation-level/
- https://akasai.space/what-is-isolation-level/
- https://nesoy.github.io/articles/2019-05/Database-Transaction-isolation
- http://wiki.gurubee.net/pages/viewpage.action?pageId=21200923
