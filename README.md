# praktikum_module_4
Итоговый проект четвертого модуля Apache Kafka (Яндекс Практикум).

Задание 1.
1. kafka-topics.sh --bootstrap-server localhost:9092 --topic balanced_topic --create --partitions 8 --replication-factor 3
2. Topic: balanced_topic   TopicId: Z1BmVOptSkOVUS7269CjvQ PartitionCount: 8       ReplicationFactor: 3    Configs:
   Topic: balanced_topic   Partition: 0    Leader: 2       Replicas: 2,0,1 Isr: 2,0,1
   Topic: balanced_topic   Partition: 1    Leader: 0       Replicas: 0,1,2 Isr: 0,1,2
   Topic: balanced_topic   Partition: 2    Leader: 1       Replicas: 1,2,0 Isr: 1,2,0
   Topic: balanced_topic   Partition: 3    Leader: 1       Replicas: 1,0,2 Isr: 1,0,2
   Topic: balanced_topic   Partition: 4    Leader: 0       Replicas: 0,2,1 Isr: 0,2,1
   Topic: balanced_topic   Partition: 5    Leader: 2       Replicas: 2,1,0 Isr: 2,1,0
   Topic: balanced_topic   Partition: 6    Leader: 2       Replicas: 2,1,0 Isr: 2,1,0
   Topic: balanced_topic   Partition: 7    Leader: 1       Replicas: 1,0,2 Isr: 1,0,2

3. reassignment.json в отдельном файле.
4. kafka-reassign-partitions.sh --bootstrap-server localhost:9092 --reassignment-json-file /tmp/reassignment.json --execute
5. Current partition replica assignment

{"version":1,"partitions":[{"topic":"balanced_topic","partition":0,"replicas":[2,0,1],"log_dirs":["any","any","any"]},{"topic":"balanced_topic","partition":1,"replicas":[0,1,2],"log_dirs":["any","any","any"]},{"topic":"balanced_topic","partition":2,"replicas":[1,2,0],"lo
g_dirs":["any","any","any"]},{"topic":"balanced_topic","partition":3,"replicas":[1,0,2],"log_dirs":["any","any","any"]},{"topic":"balanced_topic","partition":4,"replicas":[0,2,1],"log_dirs":["any","any","any"]},{"topic":"balanced_topic","partition":5,"replicas":[2,1,0],"
log_dirs":["any","any","any"]},{"topic":"balanced_topic","partition":6,"replicas":[2,1,0],"log_dirs":["any","any","any"]},{"topic":"balanced_topic","partition":7,"replicas":[1,0,2],"log_dirs":["any","any","any"]}]}

Save this to use as the --reassignment-json-file option during rollback
Successfully started partition reassignments for balanced_topic-0,balanced_topic-1,balanced_topic-2,balanced_topic-3,balanced_topic-4,balanced_topic-5,balanced_topic-6,balanced_topic-7

docker stop kafka-1

Topic: balanced_topic   TopicId: Z1BmVOptSkOVUS7269CjvQ PartitionCount: 8       ReplicationFactor: 3    Configs:
Topic: balanced_topic   Partition: 0    Leader: 0       Replicas: 0,1,2 Isr: 2,0
Topic: balanced_topic   Partition: 1    Leader: 2       Replicas: 1,2,0 Isr: 0,2
Topic: balanced_topic   Partition: 2    Leader: 2       Replicas: 2,0,1 Isr: 2,0
Topic: balanced_topic   Partition: 3    Leader: 0       Replicas: 0,1,2 Isr: 0,2
Topic: balanced_topic   Partition: 4    Leader: 0       Replicas: 0,1,2 Isr: 0,2
Topic: balanced_topic   Partition: 5    Leader: 2       Replicas: 1,2,0 Isr: 2,0
Topic: balanced_topic   Partition: 6    Leader: 2       Replicas: 2,0,1 Isr: 2,0
Topic: balanced_topic   Partition: 7    Leader: 0       Replicas: 0,1,2 Isr: 0,2

Лидеры реплик теперь находятся только на брокерах 0 и 2, т.к. брокер 1 остановлен.

docker start kafka-1

Topic: balanced_topic   TopicId: Z1BmVOptSkOVUS7269CjvQ PartitionCount: 8       ReplicationFactor: 3    Configs:
Topic: balanced_topic   Partition: 0    Leader: 0       Replicas: 0,1,2 Isr: 2,0,1
Topic: balanced_topic   Partition: 1    Leader: 2       Replicas: 1,2,0 Isr: 0,2,1
Topic: balanced_topic   Partition: 2    Leader: 2       Replicas: 2,0,1 Isr: 2,0,1
Topic: balanced_topic   Partition: 3    Leader: 0       Replicas: 0,1,2 Isr: 0,2,1
Topic: balanced_topic   Partition: 4    Leader: 0       Replicas: 0,1,2 Isr: 0,2,1
Topic: balanced_topic   Partition: 5    Leader: 2       Replicas: 1,2,0 Isr: 2,0,1
Topic: balanced_topic   Partition: 6    Leader: 2       Replicas: 2,0,1 Isr: 2,0,1
Topic: balanced_topic   Partition: 7    Leader: 0       Replicas: 0,1,2 Isr: 0,2,1

После запуска брокер 1 появился среди Isr, но не в лидерах.

Запускам выбор лидера
kafka-leader-election.sh --bootstrap-server localhost:9092 --election-type PREFERRED --all-topic-partitions

Topic: balanced_topic   TopicId: Z1BmVOptSkOVUS7269CjvQ PartitionCount: 8       ReplicationFactor: 3    Configs:
Topic: balanced_topic   Partition: 0    Leader: 0       Replicas: 0,1,2 Isr: 2,0,1
Topic: balanced_topic   Partition: 1    Leader: 1       Replicas: 1,2,0 Isr: 0,2,1
Topic: balanced_topic   Partition: 2    Leader: 2       Replicas: 2,0,1 Isr: 2,0,1
Topic: balanced_topic   Partition: 3    Leader: 0       Replicas: 0,1,2 Isr: 0,2,1
Topic: balanced_topic   Partition: 4    Leader: 0       Replicas: 0,1,2 Isr: 0,2,1
Topic: balanced_topic   Partition: 5    Leader: 1       Replicas: 1,2,0 Isr: 2,0,1
Topic: balanced_topic   Partition: 6    Leader: 2       Replicas: 2,0,1 Isr: 2,0,1
Topic: balanced_topic   Partition: 7    Leader: 0       Replicas: 0,1,2 Isr: 0,2,1

Теперь в результатах появился брокер 1 среди лидеров.

