# standalone-mirror

This docker image allow you to mirror Avro, String/JSON or Byte data from a Source to Target Kafka Clusters with one
application. <br />
*You don't need anymore to create a dedicated mirror instance for each DATA TYPE.* <br />

***IMPORTANT***  : <br />

* **Until this current version, only String KAFKA message key is accepted => other types will be soon available** <br />
* **Json SCHEMA & PROTOBUF will be available soon too**

https://hub.docker.com/r/kafkaetech/standalone-mirror

## Configurations

### Mandatory Configs

| Config                                   |  Version  | Description                                                                                                                                                                                                                                | Default                       |
|:-----------------------------------------|:---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------|
| SOURCE_BOOTSTRAP_SERVER                  | `>=0.0.1` | Bootstrap server of source Kafka cluster                                                                                                                                                                                                   | localhost:9092                |
| SOURCE_GROUP_ID                          | `>=0.0.1` | The group ID to be used by the consumer on source Kafka cluster                                                                                                                                                                            | `avro-mirror-group-id`        |
| SOURCE_SCHEMA_REGISTRY_URL               | `>=0.0.1` | Optional (from `1.0.1`) : The Schema Registry URL of the source Kafka cluster                                                                                                                                                              |                               |
| SOURCE_WHITELIST_TOPICS                  | `>=0.0.1` | The comma separated list of the topics to be mirrored from the source cluster. Data types on source topics could be : AVRO, String (simple text or JSON) or Bytes. Before 1.0.0 version, only AVRO is allowed                              | `input_topic_1,input_topic_2` |
| TARGET_BOOTSTRAP_SERVER                  | `>=0.0.1` | Bootstrap server of the target Kafka cluster                                                                                                                                                                                               | localhost:9092                |
| TARGET_SCHEMA_REGISTRY_URL               | `>=0.0.1` | Optional (from `1.0.1`): The Schema Registry URL of the target Kafka cluster                                                                                                                                                               |                               |
| TARGET_TOPIC_PREFIX                      | `>=0.0.1` | The default prefix to be used on the target cluster. Ex: If the source topic is `input_topic` and the prefix is `west1-` the target topic will be `west1-input_topic`                                                                      |                               |
| TARGET_AUTO_CREATE_TOPICS                | `>=1.1.0` | Create Topics on target clusters if not exists                                                                                                                                                                                             | `false`                       |
| TARGET_CREATE_TOPICS_IDENTICAL_AS_SOURCE | `>=1.1.0` | If `TARGET_AUTO_CREATE_TOPICS=true` and this config is set to `true`, then, the auto created topics on target cluster will has the same configs (partitions, replication-factor and cleanup-policy) as those to mirror from source cluster | `true`                        |
| TARGET_TOPICS_REPLICATION_FACTOR         | `>=1.1.0` | If `TARGET_AUTO_CREATE_TOPICS=true` && `TARGET_CREATE_TOPICS_IDENTICAL_AS_SOURCE=false`, then, this replication-factor value will be used for the auto-creation of topics in target cluster                                                | `1`                           |
| TARGET_TOPICS_PARTITIONS                 | `>=1.1.0` | If `TARGET_AUTO_CREATE_TOPICS=true` && `TARGET_CREATE_TOPICS_IDENTICAL_AS_SOURCE=false`, then, this partition number will be used for the auto-creation of topics in target cluster                                                        | `1`                           |
| TARGET_TOPICS_CLEANUP_POLICY             | `>=1.1.0` | If `TARGET_AUTO_CREATE_TOPICS=true` && `TARGET_CREATE_TOPICS_IDENTICAL_AS_SOURCE=false`, then, this cleanup-policy will be used for the auto-creation of topics in target cluster                                                          | `delete`                      |
| TARGET_HEADERS_MIRROR_ENABLED            | `>=1.1.0` | Enabling the replication of Kafka headers to target cluster                                                                                                                                                                                | `true`                        |

### Optional Configs

To override any configuration on the consumer of the Source Kafka Cluster, you have just to add a new Env variable
prefixed by `SOURCE_CONSUMER_`.
Ex:
If you would like to override the `auto.offset.reset` to `earliest` => Your Env variable will look like that :

```shell
SOURCE_CONSUMER_AUTO_OFFSET_RESET=earliest
```

To override any configuration on the producer of the Target Kafka Cluster, you have just to add a new Env variable
prefixed by `TARGET_PRODUCER_`.
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
