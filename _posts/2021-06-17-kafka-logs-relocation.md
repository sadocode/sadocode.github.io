---
layout:	post
title:	"kafka log.dirs 변경"
date:	2021-06-17
feature:	/images/git/006_kafka_logs_relocation
excerpt:	"How to relocation log.dirs in kafka"
tag:
- kafka
comments: true
---


> 최근에 회사에서 카프카서버를 구축했어요. 몇가지 계속 자잘한 에러가 발생하고 있어서, 하나씩 생각날때마다 적어두려고 합니다.

<br>

## log.dirs relocation
log.dirs는 kafka의 토픽, 파티션 등의 정보를 저장하는 path를 의미합니다.
<br>
아래 순서대로 작업하면, 데이터 손실이나 에러로그 없이 카프카를 바로 실행할 수 있어요. 
 
<br>

### step 1. 카프카 서버 정지

~~~
kafka/bin/kafka-server-stop.sh		// 기본 명령어
systemctl stop kafka-server.service	// systemctl 등록했을 경우
~~~

<br>

### step 2. 설정 변경

~~~
vi kafka/config/server.properties

#log.dirs=/data		// 기존의 log.dirs 주석 또는 삭제
log.dirs=/root/data/kafka 	// logs.dir을 원하는 path로 변경.
~~~

<br>

### step 3. dir 변경

~~~
mv /data /root/data/kafka	// /data에 저장되어있던 카프카 데이터를 /root/data/kafka로 변경
~~~

<br>

### step 4. 카프카 재실행

~~~
kafka/bin/kafka-server-start.sh	// 기본 명령어
systemctl restart kafka-server.service	// systemctl 등록했을 경우
~~~
