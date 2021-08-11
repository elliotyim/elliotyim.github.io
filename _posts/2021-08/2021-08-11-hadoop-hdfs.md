---
layout: post
title: [WIP] Hadoop (1) - HDFS
description: About
image: /assets/images/hadoop/HadoopLogo.png
date: 2021-08-11 9:30
---

# [WIP] Hadoop (1) - HDFS

# Hadoop

![HadoopEcosystem](/assets/images/hadoop/HadoopEcosystem.png)

- 대규모 데이터 셋을 여러 대의 컴퓨터 클러스터에서 분산처리할 수 있도록 해주는 아파치에서 만든 빅데이터 관련 프레임워크
- 코어 컴포넌트인 HDFS와 MapReduce로부터 시작하여 Hadoop Ecosystem을 구성하게 되었다.

# HDFS (Hadoop File System)

![HDFS](/assets/images/hadoop/HDFSNameNode.jfif)

- Apache Nutch 웹 서치 엔진 프로젝트에서 시작한 대량의 서로 다른 타입의 데이터 셋을 저장할 수 있게 만들어 주는 분산 파일 시스템으로 유닉스 운영채제에 기반을 두는 일련의 표준 운영체제 인터페이스인 POSIX의 기준에 좀 더 유연하게 만들어졌다
- 결함에 강하고(fault-tolerant) 저 비용의 하드웨어에 맞게 설계 되어 있다고 한다
- HDFS는 NameNode와 DataNode가 있다

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
-

## Name Node

- 메인 노드로 실제 데이터가 아닌 로그 파일 또는 목차와도 같은 메타데이터를 저장한다.

# References

- http://blog.newtechways.com/2017/10/apache-hadoop-ecosystem.html
- https://m.blog.naver.com/acornedu/222069158703
- https://www.edureka.co/blog/hadoop-ecosystem
- https://www.linkedin.com/pulse/analysis-hadoop-hdfs-distributed-file-system-amit-kriplani
- https://hadoop.apache.org/docs/r3.1.1/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
