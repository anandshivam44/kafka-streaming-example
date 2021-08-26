## For the sake of Ease
Set Work Directory in all terminal session
```bash
export WORK_DIR="/kafka-streaming-example"
```

## Install Kafak

cd /tmp && wget https://apachemirror.wuchna.com/kafka/2.8.0/kafka_2.13-2.8.0.tgz
cd $WORK_DIR/kafka_2.13-2.8.0 && tar -xzvf /tmp/kafka_2.13-2.8.0.tgz
cd kafka_2.13-2.8.0

## Start Services
TODO: missing chmod +x
* Start Zookeeper in new terminal
```
cd $WORK_DIR/kafka_2.13-2.8.0 && bin/zookeeper-server-start.sh config/zookeeper.properties
```

* Start Kafka Server in new terminal
```
cd $WORK_DIR/kafka_2.13-2.8.0 && bin/kafka-server-start.sh config/server.properties
```

## Basic Kafka Interactions

* Open new terminal and demo the following.
* Create a new topic

```bash
bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

* Get Topic Details

```bash
bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```

* Publish events to kafka topic

```bash
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

Do `Ctrl+C` once you are done sending the events.

* Read from Kafka topic

```bash
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```


## Run the streaming example:
TODO: Add git clone 
TODO: Add proper path with variables
Source: https://github.com/bbejeck/kafka-streams

* NOTE: First time only, create the required topics.

```bash
cd $WORK_DIR/kafka-streams && ./bin/create-topics.sh $WORK_DIR/kafka_2.13-2.8.0 localhost 2181
```
* Download the JSON data generator from here
https://github.com/everwatchsolutions/json-data-generator/releases  
* Then copy the json config files to json generator conf directory
```bash
cp streaming-workflows/* <dir>/json-data-generator-1.2.0/conf
```
* Run the JSON data generator which generates purchase orders and publishes it to topic.

```bash
cd $WORK_DIR/json-data-generator-1.4.1 && java -jar json-data-generator-1.4.1.jar purchases-config.json
```

* Run the streams which processes the orders.

```bash
cd $WORK_DIR/kafka-streams && ./gradlew runPurchaseProcessor
cd $WORK_DIR/kafka-streams && ./gradlew runPurchaseStreams
```

* Check the output topics
* 

```bash
cd $WORK_DIR/kafka_2.13-2.8.0 && bin/kafka-console-consumer.sh --topic purchases --bootstrap-server localhost:9092

cd $WORK_DIR/kafka_2.13-2.8.0 && bin/kafka-console-consumer.sh --topic rewards --bootstrap-server localhost:9092

cd $WORK_DIR/kafka_2.13-2.8.0 && bin/kafka-console-consumer.sh --topic patterns --bootstrap-server localhost:9092
```
