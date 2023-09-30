# redis-stream

https://www.youtube.com/watch?v=S4mqhH6jdHw&t=2496s

![17](https://github.com/sachit914/redis-stream/assets/137917052/4779fe49-45e3-4ad2-98d6-5d2937883e1c)



## create redis stream

```
XADD stream1 * personid 1001 name mike
```

```
XADD stream1 * personid 1002 name jack
```

* means server will create unique message id

  message id =151213123456-0

  151213123456is time in millisecond

  0 is sequence number

  
![18](https://github.com/sachit914/redis-stream/assets/137917052/b7881614-8899-44ed-9f30-425ca72d4d3b)


## querry streams

1) getting lastest data through consumer
2) get data based on time range
3) scanning data based on message ids


### time range querries 


```
XRANGE stream1 t1 t2
```

```
XRANGE stream1 - +
```

  - and + stands for get all message from start to end


### message id querries  

from start of time stamp to end of stream
```
XRANGE stream1 t1 +
```

get based on message id

```
XRANGE stream1 id +
```

### getting data by consumer

![19](https://github.com/sachit914/redis-stream/assets/137917052/c2843a3d-3fc2-4fb3-a4b0-1e03afaff3a7)



consumer group  28:20


consumer group is used to distribute the workload so that all the event wont go to one consumer
so parallel processing can be done

In summary, consumer groups in Redis Streams allow for distributed message processing across multiple consumers, ensuring efficiency, fault-tolerance, and parallelism.

we are running multiple consumer to perform task parallely


```
XREAD BLOCK 0 streams stream1 $
```

![21](https://github.com/sachit914/redis-stream/assets/137917052/e0b90b82-b964-4aae-b629-d8e980063c31)


```
XGROUP Create stream1 group1 $
```
![22](https://github.com/sachit914/redis-stream/assets/137917052/6a9e8089-fdb6-4548-9344-e3321813a100)


reading messages via consumer group

```
XREADGROUP Group group1 c1 streams stream1
```
```
XREADGROUP Group group1 c2 streams stream1
```
![23](https://github.com/sachit914/redis-stream/assets/137917052/e5d08776-56c5-4af4-a2cb-413d983de42f)


## Failover Handling     40:33 .00
