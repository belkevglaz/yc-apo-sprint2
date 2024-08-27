## Честь 2. MongoDb Sharding

Запускаем docker compose
```shell
docker compose down --remove-orphans -v && docker compose up -d
```

Инициализируем кластер redis
```shell
docker exec -it redis_1 sh -c "echo 'yes' | redis-cli --cluster create redis_1:6379 redis_2:6379 redis_3:6379 redis_4:6379 redis_5:6379 redis_6:6379 --cluster-replicas 1"
```

Замерим среднее время ответа
```shell
#!/bin/bash

url="http://0.0.0.0:8080/helloDoc/users"
total_requests=10
total_time=0

for ((i = 1; i <= total_requests; i++)); do
  time=$(curl -o /dev/null -s -w "%{time_total}" $url)
  echo "Request $i: $time seconds"
  total_time=$(echo "$total_time + $time" | bc)
done

average_time=$(echo "scale=4; $total_time / $total_requests" | bc)
echo "Average time: $average_time seconds"

```