# standalone-mirror

This docker image allow you to mirror Avro, String/JSON or Byte data from a Source to Target Kafka Clusters with one application. <br />
*You don't need anymore to create a dedicated mirror instance for each DATA TYPE.* <br />

***IMPORTANT***  : <br />
* **Until this current version, only String KAFKA message key is accepted => other types will be soon available** <br />
* **Json SCHEMA & PROTOBUF will be available soon too**

https://hub.docker.com/r/kafkaetech/standalone-mirror

## Configurations
### Mandatory Configs
| Config | Description | Default |
|----------|:-------------:|------:|
| SOURCE_BOOTSTRAP_SERVER | Bootstrap server of source Kafka cluster | localhost:9092 |
| SOURCE_GROUP_ID | The group ID to be used by the consumer on source Kafka cluster | `avro-mirror-group-id` |
| SOURCE_SCHEMA_REGISTRY_URL | Optional (from `1.0.1`) : The Schema Registry URL of the source Kafka cluster |  |
| SOURCE_WHITELIST_TOPICS | The comma separated list of the topics to be mirrored from the source cluster. Data types on source topics could be : AVRO, String (simple text or JSON) or Bytes. Before 1.0.0 version, only AVRO is allowed | `input_topic_1,input_topic_2` |
| TARGET_BOOTSTRAP_SERVER | Bootstrap server of the target Kafka cluster | localhost:9092 |
| TARGET_SCHEMA_REGISTRY_URL | Optional (from `1.0.1`): The Schema Registry URL of the target Kafka cluster |  |
| TARGET_TOPIC_PREFIX | The default prefix to be used on the target cluster. Ex: If the source topic is `input_topic` and the prefix is `west1-` the target topic will be `west1-input_topic` | |
|IGNORE_PRODUCING_ERROR|Log and ignore errors on producing messages to target cluster|true|

### Optional Configs
To override any configuration on the consumer of the Source Kafka Cluster, you have just to add a new Env variable prefixed by `SOURCE_CONSUMER_`.
Ex:
If you would like to override the `auto.offset.reset` to `earliest` => Your Env variable will look like that :
```shell
SOURCE_CONSUMER_AUTO_OFFSET_RESET=earliest
```

To override any configuration on the producer of the Target Kafka Cluster, you have just to add a new Env variable prefixed by `TARGET_PRODUCER_`.
Ex:
If you would like to override the `compression.type` to `gzip` => Your Env variable will look like that :
```shell
TARGET_PRODUCER_COMPRESSION_TYPE=gzip
```

## Execution
### Docker command line

https://hub.docker.com/r/kafkaetech/standalone-mirror

```shell
 docker run -d -e SOURCE_BOOTSTRAP_SERVER=source-cluster:9092      \
 -e SOURCE_GROUP_ID=my-group-id                                    \
 -e SOURCE_SCHEMA_REGISTRY_URL=http://schema-registry:8081         \
 -e SOURCE_WHITELIST_TOPICS=avro-topic1,string-topic2,bytes-topic3 \
 -e TARGET_BOOTSTRAP_SERVER=target-cluster:9092                    \
 -e TARGET_SCHEMA_REGISTRY_URL=http://target-cluster-schema-registry:8081                               \
 -e TARGET_TOPIC_PREFIX=mirror_                                    \
 -e IGNORE_PRODUCING_ERROR=false                                   \
 --name standalone-mirror   kafkaetech/standalone-avro-mirror:1.0.0
```
