# KSM
This is a standalone application that make easier the implementation of mirroring

## v1.2.0
1) By default, for each topic mirrored from Kafka source cluster (`SOURCE_WHITELIST_TOPICS`), the topic in the target cluster will has the same name or prefixed by `TARGET_TOPIC_PREFIX`. <br />
This config `SOURCE_WHITELIST_TOPICS` has been enhanced to allow customizing the name of topic in target cluster for each source topic using this syntax : <br />
```yaml
SOURCE_WHITELIST_TOPICS=source-input-topic-1:target_input_topic_1,source-input-topic-2:target_input_topic_2
```
This will give us this flow of mirroring : <br />
* Kafka source cluster (topic: source-input-topic-1) &rarr;  Kafka target cluster (topic: target_input_topic_1)
* Kafka source cluster (topic: source-input-topic-2) &rarr;  Kafka target cluster (topic: target_input_topic_2)

2) In some observed use cases, the data in Kafka source cluster is written used a custom Partitioner, and sometimes multiple custom Partitioner are used. so mirroring will cause a different behaviour in target cluster. <br />
To avoid creation a custom connector or application for each use case, KSM (Kafka standalone mirror) resolve it with only one config to enable `TARGET_USER_SAME_SOURCE_PARTITION`

## v1.1.0
1) Kafka headers could be replicated with `TARGET_HEADERS_MIRROR_ENABLED` that is **enabled** by default
2) Auto create topics in target cluster is possible now when activating `TARGET_AUTO_CREATE_TOPICS` (**disabled** by default)

## V1.0.1
1) Enhancement : setting the `SOURCE_SCHEMA_REGISTRY_URL` and `TARGET_SCHEMA_REGISTRY_URL` as Optional

## V1.0.0
1) Auto-detect Kafka message type from source cluster and use the same type in target cluster
2) Supported Types :
   * AVRO
   * String/JSON
   * Bytes
3) Use a prefix on target Kafka cluster topics
4) 2 possible type of error handling strategy :
   * Log & Continue, using `IGNORE_PRODUCING_ERROR=true`
   * Log & Fail, using `IGNORE_PRODUCING_ERROR=false`
