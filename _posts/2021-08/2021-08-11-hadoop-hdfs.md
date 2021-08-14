---
layout: post
title: Hadoop (1) - HDFS
description: Hadoop Ecosystems
image: /assets/images/hadoop/HadoopLogo.png
date: 2021-08-11 9:30
---

# Hadoop (1) - HDFS

# Hadoop

![HadoopEcosystem](/assets/images/hadoop/HadoopEcosystem.png)

- 대규모 데이터 셋을 여러 대의 컴퓨터 클러스터에서 분산처리할 수 있도록 해주는 아파치에서 만든 빅데이터 관련 프레임워크
- 코어 컴포넌트인 HDFS와 MapReduce로부터 시작하여 Hadoop Ecosystem을 구성하게 되었다.

# HDFS (Hadoop File System)

![HDFS](/assets/images/hadoop/HDFSNameNode.jfif)

- Apache Nutch 웹 서치 엔진 프로젝트에서 시작한 대량의 서로 다른 타입의 데이터 셋을 저장할 수 있게 만들어 주는 분산 파일 시스템으로 유닉스 운영채제에 기반을 두는 일련의 표준 운영체제 인터페이스인 POSIX의 기준에 좀 더 유연하게 만들어졌다
- 결함에 강하고(fault-tolerant) 저 비용의 하드웨어에 맞게 설계 되어 있다고 한다
- HDFS는 Master인 NameNode와 Slave인 DataNode들로 구성되어 있다

## HDFS의 목적

- 하드웨어 결함 감지
  - 수 천개의 서버 머신을 사용할 수 있는 HDFS에서 일부 결함의 가능성은 곧바로 기능장애로 이어지므로, 결함의 감지는 빠르고 자동적으로 회복하는 것이 HDFS 아키텍처의 목표이다
- 데이터 접근
  - 데이터 접근에 걸리는 지연시간 보다는 높은 데이터 처리량에 중점을 두어 일반적인 파일 시스템과는 다르다
- 대용량 데이터 셋
  - HDFS에서 돌아가는 어플리케이션들은 최소 기가바이트에서 테라바이트까지 큰 파일들을 다루기 때문에 높은 데이터 전송대역폭과 수 백개 노드 규모의 클러스터를 지원해야 한다
- 단순하고 일관성 있는 모델
  - HDFS는 write-once-read-many 모델을 표방하기 때문에 파일이 한 번 쓰여지면 그 파일 끝에 append를 하는 것을 제외하고는 수정을 할 수 없도록 잠긴다
  - 반드시 끝에 append만 허용되고 임의의 지점에 append 할 수는 없다
    - 이를 통해 데이터의 일관성과 높은 데이터 처리량이 보장된다
    - MapReduce와 웹 크롤러 어플리케이션이 이런 모델과 사용하기에 적합하다
- 연산의 이동이 데이터의 이동보다 더 코스트가 적다
  - 데이터가 크면 클 수록 데이터를 옮기는 것보다 연산을 요청 받아 처리하는 편이 네트워크의 혼잡도를 최소화하고 전체적인 처리량을 늘리기 때문에 더 효율적이다

## Name Node

- 메인 노드로 실제 데이터가 아닌 로그 파일과 데이터 블록의 수, 파일 이름, 파일 경로, 블록 ID, 블록의 위치, 레플리카의 수, slave 설정 값 등의 메타데이터를 메모리에 저장한다.
- HDFS의 메타데이터는 2개의 파일로 관리된다
  - Edit Log: 파일 시스템에서 일어나는 모든 변경 기록
  - FS Image
    - 매핑된 파일 블록들과 파일 시스템 속성을 포함한 전체 파일 시스템 네임스페이스가 저장된다
    - 다수의 FS Image 복사본과 EditLog를 관리하도록 설정하여 파일 깨짐으로 인한 기능 장애에 대처할 수 있다
      - 이 때의 업데이트 작업은 비동기적으로 일어난다
    - Name Node는 주기적으로 Data Node로 부터 health check 신호와 파일 블록 report를 받는다.
- 실제로 데이터를 저장하는 것은 Data Node 이지만 Name Node를 통해 명령을 내린다

## Data Node

- 모든 데이터는 Data Node에 저장된다. 이 Data Node들을 분산 환경의 표준 장비(일반적인 데스크탑이나 노트북 같은 장비들)를 사용하여 매우 저렴하게 구성할 수 있다.
- Data Node는 Name Node의 요청에 따라 데이터의 읽기, 쓰기, 생성, 삭제, 그리고 데이터 블록의 레플리카를 만든다
- Data Node는 파일들을 같은 폴더에 두지 않는데, 이는 로컬 파일 시스템이 한 폴더에 있는 대량의 파일들을 효율적으로 지원하지 않기 때문이다
- Data Node는 주기적으로 health check 신호와 해당 노드에 저장되어 있는 파일과 블록에 대한 정보를 보낸다
- Data Node가 기동되면 Name Node에게 자신이 가지고 있는 파일 블록들의 리스트를 알리는데, 이 때 Data Node가 10분 간격으로 아무런 신호를 보내지 않는다면 Name Node는 해당 Data Node가 죽었다고 간주하고 다른 Data Node에게 replica를 만들도록 지시한다

## Data Replication

- HDFS는 각 파일을 연속적인 블록으로 저장하는데 이 블록들은 결함 내성을 위해 복제(replicated)된다
- 마지막 블록을 제외한 파일 하나를 이루는 모든 블록의 사이즈는 같다
- Replication 속성은 파일 생성 시에 지정할 수 있고, 추후에 변경도 가능하다
- 새로운 파일이 만들어지면 Name Node는 Replica Target Data Node들의 리스트를 조회하고 사용자로 하여금 첫번째 Data Node에 파일을 쓸 수 있도록 해준다.
- 사용자가 파일을 쓰면 첫번째 Data Node는 데이터 블록들을 받아서 자신의 로컬 저장소에 저장하고, 이 데이터 블록들을 두번째 Data Node에 전송하고 이를 설정된 특정 replica의 수 만큼 반복한다.
- 파일 쓰기가 완료되면 마지막 Data Node로 부터 ACK 메시지가 전달되고 첫 번째 Data Node 까지 온 후 Name Node에 전달되면 해당 트랜잭션은 commit되어 성공적으로 종료된다

## Rack Awareness

- Rack 단위의 장애에 대해 최대한 가용성을 높이기 위해 데이터 블록의 복제본을 관리할 때 복제본이 한 군데에 몰려 있지 않도록 관리 복제본 개수가 3개인 경우 두 개는 같은 렉의 다른 노드에 저장하고 나머지 하나는 다른 렉이 있는 노드에 저장하는 것을 말한다
- 데이터 복제시 Rack을 선택하는 문제는 데이터의 손실을 방지하는 것과 네트워크의 성능을 높이는 측면에서 아무 밀접한 관련이 있다
- 각 블록에 대한 쓰기 작업이 Rack들에 분산되기 때문에 당연히 쓰기 작업에 대한 비용이 증가한다.
- 관리 복제본 개수의 기본 값은 3개인데, 이보다 많을 경우 4번째 복제본 부터는 랙 당 복제본의 개수가 상한치인 (복제본 개수-1) / 랙 개수 + 2의 아래를 유지하면서 랜덤으로 위치시킨다.

## Check Point

- HDFS 체크 포인트의 목적은 파일 시스템의 메타데이터의 스냅샷을 찍어서 FS Image에 저장함으로써 일정한 형태의 파일 시스템 메타데이터를 보장하는 것이다
- Name Node가 구동되면 FS Image와 EditLog를 디스크에서 읽고 모든 트랜잭션을 In-memory FS Image에 적용한 뒤 이 새로운 버전의 FS Image를 디스크의 FS Image에 갱신하는데 이 작업을 checkpoint라고 한다

## Communication Protocol

- 모든 HDFS 통신 프로토콜은 TCP/IP 계층 위에서 이루어져 있다
- 사용자가 자신의 프로토콜의 사용하여 NameNode와 통신을 시작하면 DataNode도 DataNode 자신의 프로토콜을 사용하여 NameNode와 통신을 시작한다
- NameNode는 어떠한 RPC도 사용하지 않고, 그저 사용자와 DataNode들의 요청에 응답만 할 뿐이다

## NameNode High Availability (HA)

- NameNode는 동작하지 않게 되면 치명적이기 때문에 보통 튼튼한 하드웨어에 배포되며, HDFS의 고가용성 아키텍처는 2개의 NameNode들을 같은 클러스터에 active/passive 설정으로 사용할 수 있게 한다

### Active NameNode

- 클러스터 내의 사용자의 모든 작업을 핸들링 한다

### Passive NameNode (Stanby Node)

- Slave NameNode로 Active NameNode와 비슷한 데이터를 가지고 있다
- failover에 빠르게 대응하기 위해 충분한 상태(state)를 유지한다

### JournalNode

![QJM-architecture](/assets/images/hadoop/QJM-architecture.png)

- Stanby node가 Active node와 상태를 동기화 시키기 위해 JournalNode라고 불리는 분리된 daemon을 통해 Active node와 서로 통신한다
- JournalNode는 최소 3개 이상으로 구성되며, 다른 NameNode, JobTracker, Yarn ResourceManager 등의 하둡 daemon들에 배치된다
- Active node에서 어떤 Namespace가 수정될 때 마다, 주요 Journal Node들에 이 기록이 로깅된다. Stanby node는 이 수정사항을 읽을 수 있고 항상 변화를 체크하고 있는다
- failover 이벤트에서 Standby node는 그 자신이 Active 상태가 되기 전에 모든 수정 기록들을 JournalNode들에게서 읽어온다
- 빠른 failover를 제공하기 위해 Standby node는 클러스터의 블록들에 대한 정보를 최신으로 유지할 필요가 있다. 이를 위해 모든 DataNode들은 Active node와 Standby node 양쪽으로 블록 리포트와 healthcheck 메시지를 보낸다

# References

- http://blog.newtechways.com/2017/10/apache-hadoop-ecosystem.html
- https://m.blog.naver.com/acornedu/222069158703
- https://www.edureka.co/blog/hadoop-ecosystem
- https://www.linkedin.com/pulse/analysis-hadoop-hdfs-distributed-file-system-amit-kriplani
- https://hadoop.apache.org/docs/r3.1.1/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- http://deepdivetechblog.com/hadoop-hdfs/
- https://fany4017.tistory.com/51
