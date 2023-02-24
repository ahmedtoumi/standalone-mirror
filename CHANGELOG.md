# KSM
This is a standalone application that make easyier the implementation of mirroring

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