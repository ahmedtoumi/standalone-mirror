# KSM
This is a standalone application that make easyier the implementation of mirroring

## v1.1.0
* Kafka headers could be replicated with `TARGET_HEADERS_MIRROR_ENABLED` that enabled by default
* Auto create topics in target cluster is possible now when activating `TARGET_AUTO_CREATE_TOPICS` (disabled by default)

## V1.0.1
* Enhancement : setting the `SOURCE_SCHEMA_REGISTRY_URL` and `TARGET_SCHEMA_REGISTRY_URL` as Optional

## V1.0.0
* Auto-detect Kafka message type from source cluster and use the same type in target cluster
* Supported Types :
  * AVRO
  * String/JSON
  * Bytes
* Use a prefix on target Kafka cluster topics
* 2 possible type of error handling strategy :
  * Log & Continue, using `IGNORE_PRODUCING_ERROR=true`
  * Log & Fail, using `IGNORE_PRODUCING_ERROR=false`
