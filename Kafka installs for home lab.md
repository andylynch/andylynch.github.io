---
share: true
categories: kafka, docker
---

```shell 
andy@tux:~/Downloads/Ex_Files_Apache_Kafka/Exercise Files$ docker compose up
```

Compose file (compose.yaml):
```yaml
version: "2"
services:
  kafka:
    image: docker.io/bitnami/kafka:3.4
    ports:
      - "9092:9092"
    volumes:
      - "kafka_data:/bitnami"
    environment:
      - ALLOW_PLAINTEXT_LISTENER=yes
volumes:
  kafka_data:
    driver: local
```

Or for a full stack, install the Confluent distribution (in `~/hack/confluent`)
```shell
andy@tux:~/hack/confluent$ docker compose up -d
# ...
andy@tux:~/hack/confluent$ docker ps
...
```

Control centre runs at http://tux:9021/clusters  ([topic view](http://tux:9021/clusters/MkU3OEVBNTcwNTJENDM2Qg/management/topics))
JSON: tux.local:8081/
KSQLDB tux.local:8088
Broker: port 9092

#kafka #Docker 


Problem: 
https://www.confluent.io/blog/kafka-client-cannot-connect-to-broker-on-aws-on-docker-etc/


## CLI to [create topics](https://developer.confluent.io/get-started/java/#create-topic)
```shfile=../create-topic.sh
docker compose exec broker \
  kafka-topics --create \
    --topic purchases \
    --bootstrap-server localhost:9092 \
    --replication-factor 1 \
    --partitions 1
```
