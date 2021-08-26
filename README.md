## Prerequisites
* Java installed in host machines when running Kafka in Host machine
* Docker installed on Host Machine
* Java installed inside Docker container
## Getting Started
```bash
git clone https://github.com/anandshivam44/kafka-streaming-example.git
```
For the sake of ease set work directory in all terminal session and use absolute paths to avoid errors
```bash
export WORK_DIR=$(pwd)/kafka-streaming-example
```


## 1. Start Services aka Start Apache Kafka in Host Machine
#### 1.1 Start Zookeeper in new terminal
```
cd $WORK_DIR/kafka_2.13-2.8.0

bin/zookeeper-server-start.sh config/zookeeper.properties
```

#### 1.2 Start Kafka Server in new terminal
```
cd $WORK_DIR/kafka_2.13-2.8.0

bin/kafka-server-start.sh config/server.properties
```




## 2. Run the streaming example:
#### 2.1 Create the required Topics
NOTE: First time only, create the required topics.

```bash
cd $WORK_DIR/kafka-streams 

./bin/create-topics.sh $WORK_DIR/kafka_2.13-2.8.0 localhost 2181
```
#### 2.2 Json Generator
* Build the json generator container
```bash
cd $WORK_DIR/json-data-generator-1.4.1

docker build -t json-data-generator .
```
* Run the JSON data generator docker container

```bash
docker run --network="host"  -it json-data-generator
```
### 3. Build and Run the streams which processes the orders.  
    
#### 3.1 Stream: PurchaseProcessor
* Build the stream `PurchaseProcessor`

```bash
cd $WORK_DIR/kafka-streams

docker build -t stream-purchase-processor -f DockerfileRunPurchaseProcessor .
```
* Run the docker container containing stream `runPurchaseProcessor`

```bash
docker run --network="host"  -it stream-purchase-processor
```
#### 3.2 Stream: Purchase
* Build the stream `PurchaseStreams`
```bash
cd $WORK_DIR/kafka-streams

docker build -t purchase-streams -f  DockerfileRunPurchaseStreams .
```
* Run the docker container containing stream `PurchaseStreams`

```bash
docker run --network="host"  -it purchase-streams
```
## 4. Check the output topics
#### 4.1 Topic: Purchases
* Build the subscriber `Purchases`
```bash
cd $WORK_DIR/kafka_2.13-2.8.0 

docker build -t subscriber-purchases -f  DockerFileOutputTopicPurchases .
```
* Run the docker container with Topic: Purchases

```bash
docker run --network="host"  -it subscriber-purchases
```
#### 4.2 Topic: Rewards
* Build the subscriber `Rewards`
```bash
cd $WORK_DIR/kafka_2.13-2.8.0 

docker build -t subscriber-rewards -f  DockerFileOutputTopicRewards .
```
* Run the docker container with Topic: Rewards

```bash
docker run --network="host"  -it subscriber-rewards
```
#### 4.3 Topic: Patterns
* Build the subscriber `Patterns`
```bash
cd $WORK_DIR/kafka_2.13-2.8.0 

docker build -t subscriber-patterns -f  DockerFileOutputTopicPatterns .
```
* Run the docker container containing stream `PurchaseStreams`

```bash
docker run --network="host"  -it subscriber-patterns
```

