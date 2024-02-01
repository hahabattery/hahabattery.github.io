

---
layout: page
title: Redis
permalink: /datastore/redis/
---

 * netflix redis, memcached
   * dynomite is using twemproxy (https://github.com/twitter/twemproxy).


스피링 시큐리티의 Session DB 와 분산락 용도(불릿의 주문처리, 시렉스-카프카의 처리시의 순서처리 ← 이건 조금 특이한 처리이긴 함.)로 사용함.


# 참조 사이트
 * redis관련 방대한 한글자료가 있음.
   * http://www.redisgate.com/redis/configuration/redis_conf_list.php



# 파일저장방식

### RDB

 * 특정 시점에 데이터를 바이너리 파일로 저정한다. AOF 파일보다 사이즈가 작아 로딩속다가 보다 빠르다.
 * 시점은 지정하는 파일은 redis.conf의 save라는 파라미터 이다. save 조건은 여러 개를 지정 할 수 있고 어느 한 것이라도 만족한다면 실행된다.
```
save 900 1   900초(15분) 동안 1번 이상 key 변경이 발생하면 저장
save 300 10   300초(5분) 동안 10번 이상 key 변경이 발생하면 저장
save 60 10000   60초(1분) 동안 10000번 이상 key 변경이 발생하면 저장
```
 * 만약 저장하지 않으려면 save를 제거 하거나 주석처리하면 된다. 명령으로도 RDB 파일을 생성할 수 있다.
 * RDB 파일 이름 지정은 redis.conf에서 dbfilename dump.rdb 이다.


명령으로도 RDB 파일을 생성할 수 있다. BGSAVE 또는 SAVE 이다.

##### BGSAVE
 * 파일 쓰기 작업은 Child process가 생성되어 background로 실행되므로 쓰기 작업 중에도 레디스 서버는 클라이언트의 명령을 정상적으로 처리할 수 있다.
 * 동작 순서
   * Step 1: Child process를 fork() 한다.
   * Step 2: Child process는 데이터를 새 RDB temp 파일에 쓴다.
   * Step 3: 쓰기가 끝나면 기존 파일을 지우고, 이름을 변경하다.

 * Redis-cli 에서 수행시 나오는 메시지
```
127.0.0.1:6379> BGSAVE
Background saving started
```

 * info persistence 로 볼 수 있는 정보 : 필요한 데이터만 간추렸다.
```
127.0.0.1:6379> info persistence
rdb_bgsave_in_progress:1   현재 Rewrite가 진행중임을 나타냄  
rdb_last_save_time:1434352287   지난 번 RDB 파일 저장 시간  
rdb_last_bgsave_status:ok   지난 번 RDB 파일 저장 성공 여부  
rdb_last_bgsave_time_sec:7   지난 번 RDB 파일 저장에 걸린 시간  
rdb_current_bgsave_time_sec:4   현재 RDB 파일 저장 시작하고 현재까지 경과 시간  
```

 * 레디스 서버 로그
```
30854:M 16:10:16.006 * Background saving started by pid 1578
  Step 1: parent process가 child process를 fork() 했다.  
1578:C  16:10:23.782 * DB saved on disk   Step 2 완료  
1578:C  16:10:23.782 * RDB: 278 MB of memory used by copy-on-write
30854:M 16:10:23.830 * Background saving terminated with success
```

 * Step 2에서 자식 프로세스가 임시 파일을 쓰는 중에 파일명은 temp-pid.rdb 이다. 이 경우는 temp-1578.rdb 이다.

##### SAVE

 * 파일 쓰기 작업을 레디스 Main process가 직접하므로 끝날때까지 클라이언트의 명령을 처리할 수 없다.
 * 동작순서
   * Step 1: Main process가 데이터를 새 RDB temp 파일에 쓴다.
   * Step 2: 쓰기가 끝나면 기존 파일을 지우고, 새 파일로 교체한다.
 * Redis-cli 에서 수행시 나오는 메시지: 완료되면 수행 시간이 표시된다.
```
127.0.0.1:6379> SAVE
OK
(4.62s)
```
 * 레디스 서버 로그: 딱 한 줄 나온다.
```
30854:M 15 Jun 16:11:27.468 * DB saved on disk
```

##### RBD관련 redis.conf 파라미터

RDB관련 redis.conf 설정
 * stop-writes-on-bgsave-error : yes or no, default yes
   * 이값이 yes일때 레디스는 RDB 파일을 디스크에 저장하다 실패하면 모든 쓰기 요청을 거부한다. 쓰기에 문제가 발생했으니 빨리 조치 하라는 것이다. default는 yes이다. 만약 no 로 설정 했다면 저장에 실패 하더라도 모든 요청을 정상적으로 처리한다.
   * 이 파라미터는 save 이벤트에만 해당한다. BGSAVE 명령을 직접 입력했을 경우에는 해당하지 않는다. 이 경우에 쓰기는 정상적으로 처리된다.

 * rdbcompression : yes or no, default yes
   * RDB 파일을 쓸때 압축 여부를 정한다. 압축률은 그다지 높지 않다고 한다.

 * rdbchecksum : yes or no, default yes
   * RDB 파일 끝에 CRC64 checksum 값을 기록한다. default인 yes로 놓고 사용한다.

 * dbfilename dump.rdb
   * RDB 파일명을 지정하나 Path는 지정할 수 없다. Path 는 working directory 에 따른다.




### AOF

 * AOF 파일은 default로 appendonly.aof 파일에 기록 된다.
 * 입력/수정/삭제 명령이 실행 될 때 마다 기록되며 조회는 기록되지 않는다.
 * AOF는 계속 추가 기록이 된다. AOF는 계속 추가 되면 파일 사이즈가 계속 커지면 OS파일 사이즈 제한에 걸러서 기록이 중단 혹은 레디스 서버 시작이 로드 시간이 많이 걸린다. 예를들어 set명령으로 같은 key 값으로 5번 수행했다면 5번 모두 남아 있다. 이걸 해결 하고자 rewrite 설정이 있다. 만약 rewrite 를 수행하면 이전 기록은 모두 사라지고 최종 데이터만 기록된다.
 * 하지만 AOF에는 5번 모두 남아 있습니다. 다른 대표적인 명령은 INCR 입니다. INCR key가 1000번 수행되었다고 하면 메모리에는 key 1000 이 하나만 남아 있습니다. AOF에는 INCR key 명령이 1000번 기록되어 있습니다. Rewrite를 수행하면 이전 기록은 모두 사라지고 최종 데이터가 기록됩니다. 즉, INCR key 1000번은 SET key 1000 하나로 기록됩니다.
 * AOF 파일은 text 파일이므로 edit 가능합니다. 실수로 FLUSHALL 명령으로 메모리에 있는 데이터 전체를 날렸을 경우, 즉시 레디스 서버를 shutdown하고, appendonly.aof 파일에서 FLUSHALL 명령을 제거 한 후 레디스를 다시 시작하면 데이터 손실없이 DB를 살릴 수 있습니다.
 * AOF를 열어보면 마지막에 아래와 같이 3줄이 있을 것이다. 이것을 삭제하고 레디스 서버를 start하면 됩니다.
```
*1
$8
flushall
```

###### AOF관련 redis.conf설정
 * appendonly : yes or no
   * AOF 기능을 사용하거나 사용하지 않는다. yes일때만 AOF파일을 읽는다.

 * appendfilename
   * AOF파일을 지정한다. Path는 RDB와 같다. 지정할 수 없고 기본을 따른다.

 * appendfsync
   * 세가지의 방법이 있다.
   * always : 명령을 실행 할때마다 기록된다. 데이터가 손실 되지는 않으나 성능이 떨어진다.
   * everysec : 1초마다 AOF에 기록된다. 그 사이에 데이터가 유실 될수 있으나 always보다는 성능이 좋고 데이터도 가능한 많이 보존할 수 있으며 일반적으로 권장하는 방법이다.
   * no : OS 가 알아서 하는 방식이긴 하나 데이터 유실이 큽니다.

 * auto-aof-rewrite-percentage 100
   * AOF 파일 사이즈가 100% 이상 커지면 rewirte 한다. 처음에는 레디스 서버가 시작할 시점의 AOF파일 사이즈 기준으로 한다. Rewrite 하면 rewirte 후 파일 사이즈 기준으로 계산한다.

 * auto-aof-rewrite-min-size 64mb
   * AOF 파일 사이즈가 64MB이하면 rewirte 하지 않는다. 파일이 작을때 rewirte가 자주 발생하는 것을 막는다.
   * 만약 0으로 설정 했다면 rewirte를 하지 않는다.

 * Master 노드 이고 Auto-restart 기능을 사용할 때는 AOF를 사용하는 것이 안전하다.
 * Msster 노드가 자동 재시작했을때 만약 AOF를 사용하지 않아서 데이터가 없다면 slave노드들의 데이터도 날리게 된다.

- - -

###### AOF Rewrite 동작 순서
 * Step 1: Child process를 fork() 한다.
 * Step 2: Child process는 데이터를 새 AOF temp 파일에 쓴다.
 * Step 3: 동시에 Parent process는 새로운 명령을 메모리 버퍼에 기록하면서 현재 AOF 파일에도 쓴다.
 * 이렇게 하는 이유는 rewrite 작업이 실패해도 데이터를 안전하게 보존하기 위해서 이다.
 * Step 4: Child process가 fork()된 시점의 데이터 쓰기 완료(1차 쓰기)되면 Child process는 Parent Process에게 stop 시그널을 보낸다.
 * Step 5: Parent Process는 stop 시그널을 받으면, Child process가 쓰는 동안 새로 발생한 데이터을 Child process에게 보내고, Child process는 이 데이터를 받아 2차로 임시 AOF 파일에 쓴다. 쓰기가 완료되면 파일을 닫고 Parent process에게 완료 시그널을 보낸다.
 * Step 6: Parent process는 임시 AOF 파일을 열고, 2차 쓰기 동안 추가로 발생한 데이터를 AOF 파일에 쓴다. 이것을 3차 쓰기라고 하자.
 * Step 7: 3차 쓰기가 완료되면 현 AOF 파일을 삭제하고 새 파일로 교체한 다음, 새 파일에 쓰기를 시작한다.



###### BGREWRITEAOF 명령 수행 정보 확인
 * AOF Rewrite는 설정에 따라 자동으로 수행되기도 하지만 BGREWRITEAOF 명령으로 명시적으로 수행시킬 수 있다.
 * Redis-cli 에서 수행시 나타나는 메지지:
```
127.0.0.1:6379> BGREWRITEAOF
Background append only file rewriting started
```
 * info persistence 로 볼 수 있는 정보 : 필요한 데이터만 간추렸다.
```
127.0.0.1:6379> info persistence
aof_enabled:1   AOF 기능이 활성화 되어 있음: redis.conf 에 appendonly yes 일때  
aof_rewrite_in_progress:1   현재 rewrite가 진행중임을 나타낸다. 아닐때는 0  
aof_last_rewrite_time_sec:4   지난 번 rewrite 하는데 걸린 시간  
aof_current_rewrite_time_sec:6   새 파일에 rewrite 를 시작하고 현재까지 경과 시간  
aof_last_bgrewrite_status:ok   지난 번 rewrite 상태  
aof_current_size:125799616   현재 AOF 파일 사이즈  
aof_base_size:119990550   베이스 AOF 파일 사이즈, base size와 current size를 비교해서 rewrite-percentage 값 이상이되면 자동으로 rewrite 한다.  
```

 * 레디스 서버 로그
```
25947:M 12:48:09.402 * Background append only file rewriting started by pid 26370
  Step 1: parent process가 child process를 fork() 했다.  
25947:M 12:48:15.525 * AOF rewrite child asks to stop sending diffs.
  Step 4   (1차 쓰기 완료)  
26370:C 12:48:15.525 * Parent agreed to stop sending diffs. Finalizing AOF...
26370:C 12:48:15.525 * Concatenating 0.41 MB of AOF diff received from parent.
  Step 5: 1차 쓰기 중에 발생한 데이터를 다시 AOF에 추가한다. (2차 쓰기 완료)  
26370:C 12:48:15.529 * SYNC append only file rewrite performed
26370:C 12:48:15.529 * AOF rewrite: 26 MB of memory used by copy-on-write
25947:M 12:48:15.577 * Background AOF rewrite terminated with success
25947:M 12:48:15.577 * Residual parent diff successfully flushed to the rewritten AOF (0.02 MB)       Step 6: 3차 쓰기 완료  
25947:M 12:48:15.577 * Background AOF rewrite finished successfully
```


### 선택

 * AOF를 기본으로 하고, RDB를 Option으로 한다. AOF 시간 설정은 everysec로 하고, AOF Rewrite를 사용한다. AOF를 사용해도 성능에 거의 영향을 미치지 않습니다.
 * Master 노드이고 Auto-restart 기능을 사용할때는 AOF를 사용하는 것이 안전하다. Master 노드가 자동 재시작했을때 AOF 파일은 없고, 몇 분 전 RDB 파일만 있다면 slave 노드들의 데이터도 마스터와 같이 몇 분 전 RDB 파일의 데이터를 받게 된다.   마스터 자동 시작과 Persistence 기능 사용은 여기를 보세요.

##### 복구

 * 데이터 파일을 읽어 들이는 명령이 있을까요? 없습니다.
   * 레디스 서버 시작 시 읽어 들입니다.

 * AOF와 RDB 파일 양쪽이 모두 존재할 경우 어느 것을 먼저 읽어들일 까요? 그것은 redis.conf 설정에 달려 있습니다.
   * appendonly yes 인 경우에는 AOF 파일을 읽어 들입니다.
   * appendonly no 인 경우 RDB 파일을 읽어 들입니다.



# Sentinel

 * SDOWN은 센티널 인스턴스가 Master와 접속이 끊긴 경우 주관적인 다운 상태로 바뀝니다. 이것은 잠시 네트워크 순단 등으로 인해 일시적인 현상일 수 있으므로 우선 SDOWN 상태가 됩니다.
 * 그러나 SDOWN 상태인 센티널들이 많아진다면 이는 ODOWN 상태(quorum), 즉 객관적인 다운 상태로 바뀝니다. 이때부터 실질적인 페일오버(failover) 작업이 시작됩니다.
