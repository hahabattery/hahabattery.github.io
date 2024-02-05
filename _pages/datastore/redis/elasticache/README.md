---
layout: page
title: ElastiCache
permalink: /datastore/redis/elasticache/
category: datastore
author: wonchang
---


![](/images/datastore/redis/elastic-cache-01.png)
![](/images/datastore/redis/elastic-cache-02.png)
![](/images/datastore/redis/elastic-cache-03.png)
![](/images/datastore/redis/elastic-cache-04.png)
![](/images/datastore/redis/elastic-cache-05.png)
![](/images/datastore/redis/elastic-cache-06.png)
![](/images/datastore/redis/elastic-cache-07.png)
![](/images/datastore/redis/elastic-cache-08.png)
![](/images/datastore/redis/elastic-cache-09.png)
![](/images/datastore/redis/elastic-cache-10.png)
![](/images/datastore/redis/elastic-cache-11.png)


# Tips

### elasticache 비번 설정

```
aws elasticache modify-replication-group --replication-group-id test --auth-token KLSp9e8yr98agsD2 --auth-token-update-strategy ROTATE --apply-immediately
```

 * 참고) 위 명령으로 비번(AUTH 명령으로 인증을 하는)을 설정하려면, encryption in transit 옵션을 활성화 시켜야 한다.
 * **2022-11-29시점에 확인했을 때는 encryption in transit 옵션을 체크하면, 암호를 설정할 수 있는 화면이 표시되었다.**
